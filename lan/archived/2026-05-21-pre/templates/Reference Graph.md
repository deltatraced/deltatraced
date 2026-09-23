
````mermaid
graph TD

%% Settings
classDef note fill:#f9f9a6,stroke:#333,stroke-width:1px,color:#000,font-style:italic;

%% Nodes
A1[^1 vscode-dbml]
A2[^2 dbml-viewer vscode marketplace]
A3[^3 gh Durandal14/vscode-extension-dbml-viewer]

%% N2_1[A minimal working project, but not immediately supporting asm]:::note

%% Connections
A3 --> |also_on| A2
A3 --> |used_with & cites| A1

%% N3_1 -.-> |about| A3
````
