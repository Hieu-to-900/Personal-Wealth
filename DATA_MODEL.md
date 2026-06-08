# DATA MODEL (Ontology)

## 1. Core Entities & Purpose

### A. Events (The Primary Source of Truth)
- **Purpose:** To record every atomic change, action, or piece of information shared by the user in a chronological, immutable sequence.
- **Location:** `/events/`
- **Rule:** Once an event file is created, it is never edited or deleted. It serves as the root for all downstream data.

### B. Assets (Physical & Financial)
- **Purpose:** To represent items owned by the user that hold value.
- **Location:** `/assets/`
- **Structure:** Must separate Acquisition History from Valuation State.
    - *Acquisition History:* Immutable metadata (original_cost, acquisition_id, purchase_date).
    - *Valuation State:* Mutable data (current_market_value, quantity_held) updated via valuation events.

### C. Liabilities (Debts & Obligations)
- **Purpose:** To represent money owed by the user (e.g., Bank Loans, Credit Cards).
- **Location:** `/liabilities/`

### D. Receivables (Money Owed to User)
- **Purpose:** To track loans made to others and their repayment status.
- **Location:** `/receivables/`

### E. Goals (Targets & Milestones)
- **Purpose:** To define desired future states and the metrics for success.
- **Location:** `/goals/`
- **Note:** The Goal itself is an Entity. Its *Progress* is a Derived View.

### F. People (Stakeholders)
- **Purpose:** To store context about individuals involved in the user's financial life.
- **Location:** `/people/`

### G. Decisions (Strategy & Context)
- **Purpose:** To record the "Why" behind actions, capturing the logic and sentiment behind the numbers.
- **Location:** `/decisions/`

### H. Journals (Narrative & Reflection)
- **Purpose:** For non-structured thoughts, reflections, or general lifestyle context that doesn't fit into a specific category.
- **Location:** `/journals/`

---

## 2. Asset Taxonomy
To ensure aggregate reporting, Assets must follow a hierarchy:
- **Category:** (e.g., Precious Metals, Equities, Real Estate, Cash)
- **Sub-type:** (e.g., Gold SJC, FPT Stocks, Apartment, Savings Account)

---

## 3. Source of Truth & Mutability

| Data Type | Mutable? | Source of Truth | Update Mechanism |
| :--- | :--- | :--- | :--- |
| **Events** | No | Primary | New file per event (Immutable). |
| **Assets/Liabilities** | Yes | Derived from Events | Butler updates metadata after Event validation. |
| **Receivables** | Yes | Derived from Events | Butler updates status based on repayment events. |
| **Goals** | Yes | Derived from Events | Butler updates target parameters via specific goals-events. |
| **Portfolio** | No (Derived) | **Calculated View** | Aggregated sum of Assets, Liabilities, and Receivables. |
| **Goal Progress** | No (Derived) | **Calculated View** | Percentage derived from current state vs Goal entity target. |

---

## 4. Knowledge States
To support "Capture First $\rightarrow$ Refine Later," the Butler must explicitly track these states:
- **Complete:** All mandatory fields for the entity type are populated.
- **Partial:** Core identity is known, but specifics (e.g., value, quantity) are pending.
- **Unknown:** Only the existence of an entity/event is recorded; no details provided.

*Rule: Views must exclude "Unknown" values from calculations unless explicitly requested by the user.*

---

## 5. ID Strategy & Naming Conventions

### Semantic IDs
The system uses human-readable Semantic IDs to maintain a narrative flow while ensuring uniqueness.
- **Format:** `[category]_[description]` (e.g., `gold_sjc`, `fpt_stocks`, `loan_anh_minh`).
- **Uniqueness Rule:** Butler must check for existing semantic strings before assignment. If a collision occurs, append an incrementing suffix (e.g., `gold_sjc_02`).

### Folder Structure & Naming
- `/assets/`: `ASSET_[ID].md`
- `/liabilities/`: `LIAB_[ID].md`
- `/receivables/`: `REC_[ID].md`
- `/goals/`: `GOAL_[ID].md`
- `/people/`: `PERSON_[ID].md`
- `/journals/`: `JOURNAL_YYYYMMDD.md`
- `/decisions/`: `DECISION_[ID].md`
- `/events/`: `EVENT_[YYYYMMDD_HHMMSS]_[ShortDesc].md`
- `/views/`: Aggregated summaries (`portfolio.md`, `active_goals.md`, `outstanding_receivables.md`).

---

## 6. View Reproducibility & Caching
- **Reproducibility:** Every view must be strictly reconstructible from the underlying entities and event logs at any point in time.
- **Caching:** To optimize performance, snapshots of views may be cached. However, these snapshots must be refreshed whenever a new Event is validated that impacts the relevant entity or goal.
