``` mermaid
flowchart TD
    Start[Start: API Protection & Validation] --> S1{Is Swagger/OpenAPI file available?}
    
    %% Path if Swagger is available
    S1 -- Yes --> S2[Step 2: Upload to XC & Create API Definition]
    
    %% Path if Swagger is NOT available (Your new logic)
    S1 -- No --> Disc1[Agree on timeframe for Discovery]
    Disc1 --> Disc2[Devs generate significant traffic/API calls]
    Disc2 --> Disc3[XC identifies APIs via Discovery]
    Disc3 --> Disc4[Download discovered Swagger from XC]
    Disc4 --> Disc5[Devs review & Sign-off]
    Disc5 --> S2
    
    %% Standard Flow continues
    S2 --> S3[Step 3: Create API Groups]
    S3 --> S4[Step 4: LB Enforcement - Select Definition & Custom List]
    
    S4 --> S5[Step 5: Create Validation Rules]
    S5 --> S6[Step 6: Policy Staging - Set to 'Report']
    
    S6 --> FP{Perform False Positive Analysis?}
    FP -- Findings Present --> S5
    FP -- Traffic Legitimate --> S6B[Switch Action to 'Block']
    
    S6B --> S7[Step 7: Configure Fall-Through Logic]
    S7 --> FT_Final[Set Final Action: Skip or Block]
    FT_Final --> End[End: Protection Enforced]
```
