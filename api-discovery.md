``` mermaid
flowchart TD
    A[Start: API Discovery] --> B[Step 1: Identify API Applications]
    B --> C[Step 2: Navigate to HTTP LB > API Protection]
    C --> D[Step 3: Toggle API Discovery to 'Enabled']
    D --> E[Step 4: Define Learning Parameters]
    
    subgraph Parameters [Configuration Detail]
    E1[Response Filter: 200 OK or Redirects]
    E2[Retention Policy: Define duration]
    end
    
    E --> E1
    E --> E2
    E1 & E2 --> F[End: Discovery Active]
```
