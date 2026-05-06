``` mermaid
flowchart TD
    A[Start] --> B[Step 1: Application Type Assessment]

    B --> C{Is it an API?}
    C -- Yes --> D[End: No CSRF protection required]
    C -- No (Web) --> E[Step 2: Existing Application-Level Protection check]

    E --> F{Already Protected?}
    F -- Yes --> G[End: No CSRF protection required on XC platform]
    F -- No --> H[Step 3: XC Platform Action Item]

    H --> I[Apply CSRF protection on F5 XC platform]

    I --> J[Prerequisite: Configure trusted domains/referrers]
    J --> K[End]
```
