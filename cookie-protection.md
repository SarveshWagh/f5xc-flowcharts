``` mermaid
flowchart TD
    A[Start] --> B[Step 1: Cookie Type Assessment]

    B --> C{Is it a Session or Identity/Auth cookie?}
    
    C -- Yes --> D[Step 2: Existing Application-Level Protection Check]
    C -- No (Functional/Analytics) --> E[End: Protection typically not required]
    
    D --> F{Is it already hardened at the App level?}
    
    F -- Yes --> G[End: No further protection required on XC]
    F -- No --> H[Step 3: Apply cookie protection on F5 XC platform]
    
    H --> I[Prerequisite: Gather specific cookie name]
    I --> J[Prerequisite: Define target attributes: HttpOnly, Secure, SameSite]
    J --> K[End]
```
