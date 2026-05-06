``` mermaid
flowchart TD
    A[Start] --> B[Step 1: Cookie Type Assessment]

    B --> C{Is it a Session cookie?}
    C -- Yes --> D[Step 2: Existing Application-Level Protection Check]
    C -- No (Other cookie types) --> E[End: No cookie protection can be applied on XC platform]

    D --> F{Already Protected at Application Level?}
    F -- Yes --> G[End: No cookie protection required on XC platform]
    F -- No --> H[Step 3: XC Platform Action Item]

    H --> I[Apply cookie protection on F5 XC platform]

    I --> J[Prerequisite: Gather session cookie name]
    J --> K[Prerequisite: Define cookie settings: HTTP, Secure, SameSite]

    K --> L[End]
```
