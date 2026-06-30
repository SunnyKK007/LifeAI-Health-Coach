# BRAIN.MD — Decision Logic, Data Models & Governance
> Scope: Logic matrices, database relationships, and conditional rules for the platform.
> Constraint: Money-Back Outcome Guarantee logic is completely excluded.

---

## SECTION 1: CORE BIOMETRIC CLINICAL ROUTING

### 1.1 LabCorp HbA1c Eligibility Matrix
The platform employs strict conditional logic checks on incoming laboratory results to determine program entry or diversion.

```mermaid
graph TD
    Start([Incoming Lab Result Ingested]) --> Low["HbA1c < 5.7%"]
    Start --> High["HbA1c >= 5.7%"]
    
    Low --> Reject1("Reject & Divert")
    
    High --> Mid["HbA1c 5.7% - 6.4%"]
    High --> VeryHigh["HbA1c >= 6.5%"]
    
    Mid --> Unlock("Unlock Full DPP")
    VeryHigh --> Reject2("Reject & Refer")
    
    style Start fill:#f9f,stroke:#333,stroke-width:2px
    style Reject1 fill:#ff9999,stroke:#333,stroke-width:2px
    style Reject2 fill:#ff9999,stroke:#333,stroke-width:2px
    style Unlock fill:#99ff99,stroke:#333,stroke-width:2px
```

* **Logic Specifications:**
    ```python
    def evaluate_hba1c_eligibility(user_id, hba1c_percentage):
        # Category 1: Normal glycemic profile (Ineligible)
        if hba1c_percentage < 5.7:
            set_user_registration_status(user_id, status="INELIGIBLE_NOT_PREDIABETIC")
            dispatch_routing_notification(user_id, template_id="REDIRECT_LOW_RISK")
            terminate_onboarding_lifecycle(user_id)
        
        # Category 2: Confirmed Prediabetes profile (Eligible for DPP)
        elif 5.7 <= hba1c_percentage <= 6.4:
            set_user_registration_status(user_id, status="ELIGIBILITY_VERIFIED")
            provision_cohort_allocation_queue(user_id)
            unlock_program_curriculum_access(user_id)
            
        # Category 3: Confirmed Diabetes profile (Outside preventive scope)
        elif hba1c_percentage >= 6.5:
            set_user_registration_status(user_id, status="INELIGIBLE_DIABETIC_DIAGNOSIS")
            dispatch_routing_notification(user_id, template_id="REDIRECT_CLINICAL_DIABETES")
            terminate_onboarding_lifecycle(user_id)
    ```

---

## SECTION 2: INSURANCE COMPLIANCE & CMS CLAIMS STATE MACHINE

### 2.1 Medicare G-Code Milestone Lifecycle Matrix
Claims processing operates on event-driven state transitions triggered when attendance milestones or weight loss targets are recorded in the database.

| G-Code Reference | Program Lifecycle Window | Definitive Trigger Condition | Transactional Action |
| :--- | :--- | :--- | :--- |
| **G9873** | Core Phase (Months 1-6) | `Core_Attendance_Count == 1` | Generates initial CMS enrollment claim payload. |
| **G9874** | Core Phase (Months 1-6) | `Core_Attendance_Count == 4` | Generates milestone 2 core claim file. |
| **G9875** | Core Phase (Months 1-6) | `Core_Attendance_Count == 9` | Generates milestone 3 core claim file. |
| **G9878** | Maintenance Phase (Months 7-12) | `Maintenance_Attendance == 3` AND `Current_Month` within [7, 8, 9] | Generates first maintenance phase interval claim. |
| **G9879** | Maintenance Phase (Months 7-12) | `Maintenance_Attendance == 3` AND `Current_Month` within [10, 11, 12] | Generates second maintenance phase interval claim. |
| **G9880** | Cumulative Target Window | `Weight_Loss_Percentage >= 5.00` calculated from the initial baseline weight | Generates outcomes-based performance bonus payment claim. |

### 2.2 Claims Lifecycle State Machine
```mermaid
flowchart TD
    A([State: UNTRACKED]) -->|Trigger Condition Evaluated| B([State: CLAIM_QUEUED])
    B -->|EDI 837P Transmission Complete| C([State: SUBMITTED_TO_CLEARINGHOUSE])
    C --> D([State: PAID_SETTLED])
    C --> E([State: BATCH_REJECTED])
    D --> F(Log Revenue Event)
    E --> G(Raise Billing Alert Queue)
    
    style A fill:#e1f5fe,stroke:#0288d1
    style B fill:#fff3e0,stroke:#f57c00
    style C fill:#fff3e0,stroke:#f57c00
    style D fill:#e8f5e9,stroke:#388e3c
    style E fill:#ffebee,stroke:#d32f2f
    style F fill:#c8e6c9,stroke:#2e7d32
    style G fill:#ffcdd2,stroke:#c62828
```

---

## SECTION 3: CORE APPLICATION DATA SCHEMA

The database schema enforces standard relational structures to maintain clean separation of concerns across tracking, communication, and financial operations.

```mermaid
graph TD
    USER["<b>USER</b><br/>user_id (PK)<br/>cohort_id (FK)<br/>rd_id (FK)<br/>status"]
    LAB_RECORDS["<b>LAB_RECORDS</b><br/>lab_id (PK)<br/>user_id (FK)<br/>hba1c_value<br/>status"]
    WEIGHT_LOG["<b>WEIGHT_LOG</b><br/>log_id (PK)<br/>user_id (FK)<br/>weight_lbs<br/>timestamp"]
    ATTENDANCE["<b>ATTENDANCE</b><br/>att_id (PK)<br/>user_id (FK)<br/>session_num<br/>duration_min"]
    COHORT_ROOM["<b>COHORT_ROOM</b><br/>cohort_id (PK)<br/>zoom_room_id<br/>start_date"]
    BILLING_LOG["<b>BILLING_LOG</b><br/>claim_id (PK)<br/>user_id (FK)<br/>g_code<br/>status"]
    
    USER -->|has| LAB_RECORDS
    USER -->|records| WEIGHT_LOG
    USER -->|logs| ATTENDANCE
    COHORT_ROOM -->|contains| USER
    USER -->|generates| BILLING_LOG
    
    style USER fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    style LAB_RECORDS fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style WEIGHT_LOG fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    style ATTENDANCE fill:#fff3e0,stroke:#e65100,stroke-width:2px
    style COHORT_ROOM fill:#ffebee,stroke:#c62828,stroke-width:2px
    style BILLING_LOG fill:#e0f7fa,stroke:#006064,stroke-width:2px
```

### 3.1 Structural Constraints
* **`USER` Isolation:** Holds system lifecycle states, onboarding parameters, and foreign key references to the assigned `COHORT_ROOM` and `DIETITIAN` tables.
* **`ATTENDANCE` Constraint Rules:** Every session attendance verification records the duration of the connection via Zoom server webhook integration. This structure provides audit trails required for CDC National DPP data submissions.
* **`WEIGHT_LOG` Constraint Rules:** Retains all raw scale readings along with accurate time-series metrics. The database evaluates new entries against the baseline reading to compute the current weight loss percentage used to trigger outcomes-based insurance claims.

---

## SECTION 4: SYSTEM BOUNDARY & DATA GOVERNANCE RULES

### 4.1 Automated Notification Dispatch Constraints
To prevent duplicate alerts and protect the member experience, the notification engine follows a priority checklist before dispatching reminders:

```mermaid
graph TD
    Start([Execute Cron Ingestion Task]) --> Q1(Is User Status Suspended)
    
    Q1 -->|Yes| Drop[Drop Payload]
    Q1 -->|No| Q2(Is User App Session Active)
    
    Q2 -->|Yes| Badge[Update App Badge]
    Q2 -->|No| Q3(Target Activity Already Logged)
    
    Q3 -->|Yes| Kill[Kill Lifecycle]
    Q3 -->|No| Dispatch([Dispatch Remote Push Notification])
    
    style Start fill:#e1bee7,stroke:#8e24aa,stroke-width:2px
    style Drop fill:#ffcdd2,stroke:#c62828
    style Badge fill:#c8e6c9,stroke:#2e7d32
    style Kill fill:#ffcc80,stroke:#ef6c00
    style Dispatch fill:#bbdefb,stroke:#1565c0,stroke-width:2px
```

### 4.2 Security Boundaries & Privacy Constraints
* **Message Routing Access Controls:** Dietitians are isolated from cross-tenant user viewing through Row-Level Security (RLS) policies. The database restricts data access to ensure RDs can only query profiles that contain a matching `rd_id` reference.
* **PII Encryption Strategies:** Personally Identifiable Information (PII) including Insurance Card Member IDs, National Card Identifiers, and phone numbers are encrypted at the field level using cryptographically strong storage formats. These values are automatically masked within system application logging environments.
