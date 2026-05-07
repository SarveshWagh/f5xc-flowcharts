``` mermaid
flowchart TD
    A[Start] --> B[Step 1: Cookie Type Assessment]

    B --> C{Is it a Session cookie?}
    C -- Yes --> D[Step 2: Existing Application-Level Protection Check]
    C -- No (Other cookie types) --> E[End: No cookie protection can be applied on XC platform]
    D --> F{Already Protected at Application Level?}
    F -- Yes --> G[End: No cookie protection required on XC platform]
    F -- No --> H[Step 3: Apply cookie protection on F5 XC platform]
    H --> I[Prerequisite: Gather session cookie name]
    I --> J[Prerequisite: Gather cookie settings: HTTP, Secure, SameSite]
    J --> K[End]
```
