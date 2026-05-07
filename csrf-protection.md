``` mermaid
flowchart TD
    A[Start] --> B[Step 1: Application Authentication Assessment]

    B --> C{Does the app use Cookie-based Auth?}
    
    C -- No (Token/Header Auth) --> D[End: CSRF protection not required]
    C -- Yes (Web or API) --> E[Step 2: Existing Application-Level Protection check]
    
    E --> F{Is it already protected via CSRF tokens or origin/referrer validation?}
    
    F -- Yes --> G[End: No CSRF protection required on XC platform]
    F -- No --> H[Step 3: Apply CSRF protection on F5 XC platform]
    
    H --> I[Prerequisite: Gather trusted domains/referrers]
    I --> J[End]
```
