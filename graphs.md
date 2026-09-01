graph TD
    %% Estilos de nodos
    classDef person fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef movie fill:#fff9c4,stroke:#fbc02d,stroke-width:2px;

    %% Nodos de Personas y Películas
    KB["Kevin Bacon (ID: 102)"]:::person
    A13["Apollo 13 (1995)"]:::movie
    GS["Gary Sinise (ID: 641)"]:::person
    FG["Forrest Gump (1994)"]:::movie
    RW["Robin Wright (ID: 705)"]:::person

    %% Conexiones (Aristas)
    KB -->|Estelarizó en| A13
    A13 -->|Incluyó a| GS
    GS -->|Estelarizó en| FG
    FG -->|Incluyó a| RW
 
