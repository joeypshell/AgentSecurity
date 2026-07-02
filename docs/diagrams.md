# Diagrams

Last reviewed: 2026-07-02

These diagrams are written in Mermaid so they render in GitHub Markdown and can be edited as text. Standalone source files are in `diagrams/`.

## 1. Agent loop

```mermaid
flowchart TD
    U[User goal] --> C[Collect context]
    C --> P[Plan next step]
    P --> T{Need a tool?}
    T -- no --> R[Respond]
    T -- yes --> A[Request or use tool]
    A --> O[Observe result]
    O --> V{Goal met?}
    V -- no --> C
    V -- yes --> R
    O --> I{Untrusted content?}
    I -- yes --> G[Apply guardrails]
    G --> P
```

## 2. Trust boundaries

```mermaid
flowchart LR
    User[Developer] --> UI[VS Code or GitHub Copilot UI]
    UI --> Model[Model provider]
    UI --> Workspace[Workspace files]
    UI --> Terminal[Terminal]
    UI --> MCP[MCP servers]
    UI --> Web[Web content]
    Terminal --> Local[Local OS and credentials]
    MCP --> GitHub[GitHub]
    MCP --> Cloud[Cloud APIs]
    MCP --> DB[Databases]
    Workspace --> UI
    Web --> UI
    classDef danger fill:#ffe0e0,stroke:#aa0000,color:#000;
    class Terminal,MCP,Cloud,DB,Local,Web danger;
```

## 3. Secure workflow

```mermaid
flowchart LR
    A[Ask] --> B[Plan]
    B --> C{Human approval}
    C -- yes --> D[Implement in branch or worktree]
    D --> E[Tests and checks]
    E --> F[Read-only security review]
    F --> G{Human PR review}
    G -- yes --> H[Protected merge]
    C -- no --> B
    G -- changes --> D
```

## 4. MCP risk flow

```mermaid
flowchart TD
    Agent[Agent] --> Decide{Needs external system?}
    Decide -- no --> Local[Use local read/search]
    Decide -- yes --> MCP[MCP server]
    MCP --> Tools{Tool type}
    Tools -- read --> Read[Read external data]
    Tools -- write/admin --> Risk[High-risk mutation path]
    Risk --> Controls[Require allowlist, least privilege, logging, approval]
    Read --> Injection[Untrusted returned content]
    Injection --> Validate[Validate and ignore embedded instructions]
```

## 5. Control map

```mermaid
flowchart TB
    Risk[Agent risk] --> P[Prompt and instruction controls]
    Risk --> T[Tool controls]
    Risk --> R[Runtime controls]
    Risk --> G[Repository controls]
    Risk --> H[Human controls]

    P --> P1[Custom instructions]
    P --> P2[Prompt files]
    P --> P3[Custom agent role]

    T --> T1[Explicit tool list]
    T --> T2[MCP allowlist]
    T --> T3[Terminal approval]

    R --> R1[Sandbox]
    R --> R2[Network filter]
    R --> R3[Devcontainer or VM]

    G --> G1[CODEOWNERS]
    G --> G2[Branch protection]
    G --> G3[Required checks]

    H --> H1[Plan review]
    H --> H2[PR review]
    H --> H3[Incident response]
```
