🧱 LAYER 1 – Initialization & Toolchain

ID	Component	Task
T1.1	Infra	Create project scaffold: src/, agents/, tools/, workflows/, etc.
T1.2	Infra	Set up tsconfig.json with strict mode and path aliases
T1.3	Infra	Install and configure ESLint with @typescript-eslint rules
T1.4	Infra	Configure Prettier + lint/prettier integration
T1.5	Infra	Install and configure vitest, basic smoke test setup
T1.6	Infra	Initialize Git repo and add .gitignore, commit hooks


⸻

🔧 LAYER 2 – Core Framework & State

ID	Component	Task	Depends On
T2.1	Mastra	Install and configure @mastra/core, @mastra/agents, @mastra/tools	T1.1
T2.2	Mastra	Define agent registry and configuration file	T2.1
T2.3	Mastra	Define tools registry and plug into agents	T2.1
T2.4	MCP	Create initial task-kanban-mcp schema (task states, logs)	T1.1
T2.5	MCP	Implement PostgreSQL or in-memory store for task tracking	T2.4
T2.6	MCP	Add MCP task lifecycle enum and schema validator	T2.4
T2.7	MCP	Scaffold MCP server with basic WebSocket handler	T2.4, T1.2
T2.8	MCP	Add authentication layer to MCP server	T2.7
T2.9	MCP	Implement task queue with FIFO/Priority switching	T2.7


⸻

🤖 LAYER 3 – Agent Implementation

Planning Agent

ID	Component	Task	Depends On
T3.1.1	Planning	Define PlanningAgent class, register with Mastra	T2.2
T3.1.2	Planning	Configure Claude Sonnet model with Anthropic SDK	T3.1.1
T3.1.3	Planning	Implement tool: create_task	T3.1.1, T2.3
T3.1.4	Planning	Implement tool: get_project_status	T3.1.1, T2.3
T3.1.5	Planning	Implement tool: communicate_with_dev_agent	T3.1.1, T2.3
T3.1.6	Planning	Implement user message handler with intent parser	T3.1.3–T3.1.5

Background Agent

ID	Component	Task	Depends On
T3.2.1	Background	Define BackgroundAgent class, register with Mastra	T2.2
T3.2.2	Background	Configure Claude Haiku model	T3.2.1
T3.2.3	Background	Implement tool: run_tests	T3.2.1, T2.3
T3.2.4	Background	Implement tool: type_check	T3.2.1, T2.3
T3.2.5	Background	Implement tool: lint_and_format	T3.2.1, T2.3
T3.2.6	Background	Implement tool: git_commit	T3.2.1, T2.3
T3.2.7	Background	Implement tool: update_docs	T3.2.1, T2.3
T3.2.8	Background	Implement handleFileChange and action resolver	T3.2.3–T3.2.7


⸻

🔍 LAYER 4 – File System Watchers

ID	Component	Task	Depends On
T4.1	Watcher	Create FileSystemWatcher class wrapper for chokidar	T3.2.8
T4.2	Watcher	Add debounce, hashing, and event deduplication	T4.1
T4.3	Watcher	Support platform-specific backends (inotify, fsevents)	T4.1
T4.4	Watcher	Hook into BackgroundAgent via event bus	T4.2, T3.2.8


⸻

🧠 LAYER 5 – Cursor IDE Integration

ID	Component	Task	Depends On
T5.1	Cursor	Implement MCP WebSocket client inside Cursor plugin	T2.7, T1.1
T5.2	Cursor	Subscribe to PlanningAgent task topic	T5.1, T3.1.5
T5.3	Cursor	Display tasks in Cursor task panel	T5.2
T5.4	Cursor	Inject context (requirements, acceptance, patterns)	T5.3, T2.3
T5.5	Cursor	Sync status updates back to PlanningAgent	T5.3
T5.6	Cursor	Trigger file saves for BackgroundAgent on change	T5.3, T4.4
T5.7	Cursor	Add Autonomous Mode toggle	T5.3, T5.4
T5.8	Cursor	Load pattern examples and docs inline	T2.3, T5.4


⸻

🔁 LAYER 6 – Orchestration Engine

ID	Component	Task	Depends On
T6.1	Orchestration	Implement DAG-based task planner	T3.1.6
T6.2	Orchestration	Add feedback loop pattern (Background → Planning)	T3.2.8, T3.1.6
T6.3	Orchestration	Add saga-style rollback on failed commits	T3.2.6
T6.4	Orchestration	Add sequential and parallel task coordination logic	T6.1
T6.5	Orchestration	Add dead letter handling, retries, escalation	T6.3
T6.6	Orchestration	Connect to Mastra event bus for all state flows	T6.2, T6.4


⸻

📊 LAYER 7 – Observability & Scoring

ID	Component	Task	Depends On
T7.1	Telemetry	Implement task lifecycle telemetry logger	T6.6
T7.2	Telemetry	Add BackgroundAgent quality gate metrics	T3.2.8
T7.3	UI	Build Orchestration Map UI (DAG rendering)	T6.1, T7.1
T7.4	UI	Build Pattern Scoreboard UI in Cursor	T5.8, T7.2
T7.5	UI	Add rating interface for pattern usefulness	T7.4
T7.6	UI	Add feedback ingestion pipeline from developers	T7.5


⸻

🧪 LAYER 8 – Reliability Systems

ID	Component	Task	Depends On
T8.1	Reliability	Inject failure stubs into BackgroundAgent tools	T3.2.3–T3.2.7
T8.2	Reliability	Test recovery from invalid file events	T4.1, T3.2.8
T8.3	Reliability	Implement circuit breaker & scoring fallback	T6.5
T8.4	Reliability	Validate rollback & retry under edge cases	T6.3, T8.1

⸻

✅ 1. Notion-Compatible Mermaid Block (for Embed)

To use in Notion:
	•	Create a code block
	•	Set the language to mermaid
	•	Paste this directly

graph TD
  %% Phase 1 – Tooling & Infra
  subgraph Phase_1 [Phase 1: Initialization & Tooling]
    T1["T1.1: Project Scaffold"]
    T2["T1.2: tsconfig Setup"]
    T3["T1.3: ESLint Config"]
    T4["T1.4: Prettier Config"]
    T5["T1.5: Vitest Init"]
    T6["T1.6: Git Hooks"]
  end

  subgraph Phase_2 [Phase 2: Mastra & MCP Infrastructure]
    T7["T2.1: Install Mastra Core"]
    T8["T2.2: Agent Registry"]
    T9["T2.3: Tool Registry"]
    T10["T2.4: MCP Schema"]
    T11["T2.5: Persistence Layer"]
    T12["T2.6: Lifecycle Enum"]
    T13["T2.7: MCP WebSocket Server"]
    T14["T2.8: MCP Auth"]
    T15["T2.9: Task Queue Engine"]
  end

  subgraph Phase_3 [Phase 3: Agents]
    T20["T3.1.1: PlanningAgent Base"]
    T21["T3.1.2: Claude Config"]
    T22["T3.1.3: create_task"]
    T23["T3.1.4: get_status"]
    T24["T3.1.5: communicate_dev"]
    T25["T3.1.6: handleUserMessage"]
    T30["T3.2.1: BackgroundAgent Base"]
    T31["T3.2.2: Claude Config"]
    T32["T3.2.3: run_tests"]
    T33["T3.2.4: type_check"]
    T34["T3.2.5: lint_format"]
    T35["T3.2.6: git_commit"]
    T36["T3.2.7: update_docs"]
    T37["T3.2.8: handleFileChange"]
  end

  subgraph Phase_4 [Phase 4: File Watcher]
    T40["T4.1: FileSystemWatcher"]
    T41["T4.2: Debounce + Hashing"]
    T42["T4.3: Platform Hooks"]
    T43["T4.4: Hook to Agent"]
  end

  subgraph Phase_5 [Phase 5: Cursor Integration]
    T50["T5.1: MCP Client"]
    T51["T5.2: Subscribe Tasks"]
    T52["T5.3: Task Panel"]
    T53["T5.4: Inject Context"]
    T54["T5.5: Status Updates"]
    T55["T5.6: File Save Triggers"]
    T56["T5.7: Autonomous Mode"]
    T57["T5.8: Pattern Inline"]
  end

  subgraph Phase_6 [Phase 6: Orchestration]
    T60["T6.1: DAG Planner"]
    T61["T6.4: Sequential Logic"]
    T62["T6.2: Feedback Loop"]
    T63["T6.3: Rollback Engine"]
    T64["T6.5: Retry Handling"]
    T65["T6.6: Event Bus"]
  end

  subgraph Phase_7 [Phase 7: Observability & UI]
    T70["T7.1: Telemetry Logs"]
    T71["T7.2: Quality Metrics"]
    T72["T7.3: Orchestration Map"]
    T73["T7.4: Pattern Score UI"]
    T74["T7.5: Rating Interface"]
    T75["T7.6: Feedback Pipeline"]
  end

  subgraph Phase_8 [Phase 8: Reliability]
    T80["T8.1: Failure Stubs"]
    T81["T8.2: FS Event Recovery"]
    T82["T8.3: Circuit Breaker"]
    T83["T8.4: Rollback Testing"]
  end

  %% Dependencies
  T1 --> T7
  T7 --> T8 --> T9
  T10 --> T11
  T10 --> T12
  T10 --> T13 --> T14
  T13 --> T15

  T8 --> T20 --> T21
  T20 --> T22 --> T25
  T20 --> T23 --> T25
  T20 --> T24 --> T25

  T8 --> T30 --> T31
  T30 --> T32 --> T37
  T30 --> T33 --> T37
  T30 --> T34 --> T37
  T30 --> T35 --> T37
  T30 --> T36 --> T37

  T37 --> T40 --> T41 --> T43
  T40 --> T42

  T13 --> T50 --> T51 --> T52
  T52 --> T53 --> T57
  T52 --> T54
  T52 --> T55
  T52 --> T56

  T25 --> T60 --> T61 --> T65
  T37 --> T62 --> T65
  T35 --> T63 --> T64 --> T65

  T65 --> T70
  T37 --> T71
  T60 --> T72
  T57 --> T73 --> T74 --> T75

  T32 --> T80
  T40 --> T81
  T64 --> T82
  T63 --> T83


⸻

✅ 2. Phase Grouping Already Included Above
	•	Phase 1–8 headers are encoded as subgraph blocks in Mermaid.
	•	Use these to visually group tasks by sprint, milestone, or responsibility domain.

⸻

✅ 3. CSV Task Export (Kanban/Ticketing Compatible)

Here’s the CSV-style data for import into Notion, ClickUp, Linear, Trello, or Jira:

ID	Phase	Title	Depends On
T1.1	Phase 1	Project Scaffold	
T1.2	Phase 1	tsconfig Setup	T1.1
T1.3	Phase 1	ESLint Config	T1.1
T1.4	Phase 1	Prettier Config	T1.1
T1.5	Phase 1	Vitest Init	T1.1
T1.6	Phase 1	Git Hooks	T1.1
T2.1	Phase 2	Install Mastra Core	T1.1
T2.2	Phase 2	Agent Registry	T2.1
T2.3	Phase 2	Tool Registry	T2.2
T2.4	Phase 2	MCP Schema	T1.1
T2.5	Phase 2	Persistence Layer	T2.4
T2.6	Phase 2	Lifecycle Enum	T2.4
T2.7	Phase 2	MCP WebSocket Server	T2.4
T2.8	Phase 2	MCP Auth	T2.7
T2.9	Phase 2	Task Queue Engine	T2.7
T3.1.1	Phase 3	PlanningAgent Base	T2.2
T3.1.2	Phase 3	Claude Config	T3.1.1
T3.1.3	Phase 3	create_task Tool	T3.1.1
T3.1.4	Phase 3	get_status Tool	T3.1.1
T3.1.5	Phase 3	communicate_dev Tool	T3.1.1
T3.1.6	Phase 3	handleUserMessage Logic	T3.1.3–T3.1.5
T3.2.1	Phase 3	BackgroundAgent Base	T2.2
T3.2.2	Phase 3	Claude Config	T3.2.1
T3.2.3	Phase 3	run_tests Tool	T3.2.1
T3.2.4	Phase 3	type_check Tool	T3.2.1
T3.2.5	Phase 3	lint_format Tool	T3.2.1
T3.2.6	Phase 3	git_commit Tool	T3.2.1
T3.2.7	Phase 3	update_docs Tool	T3.2.1
T3.2.8	Phase 3	handleFileChange Logic	T3.2.3–T3.2.7
T4.1	Phase 4	FileSystemWatcher Base	T3.2.8
T4.2	Phase 4	Debounce & Hashing	T4.1
T4.3	Phase 4	Platform Hooks	T4.1
T4.4	Phase 4	Hook Watcher to Agent	T4.2
T5.1	Phase 5	MCP Client in Cursor	T2.7
T5.2	Phase 5	Subscribe to Tasks	T5.1
T5.3	Phase 5	Show Task Panel	T5.2
T5.4	Phase 5	Inject Task Context	T5.3
T5.5	Phase 5	Send Status Updates	T5.3
T5.6	Phase 5	File Save Triggers	T5.3
T5.7	Phase 5	Autonomous Mode Toggle	T5.3
T5.8	Phase 5	Inline Pattern Loading	T5.4, T2.3
T6.1	Phase 6	DAG Planner	T3.1.6
T6.2	Phase 6	Feedback Loop	T3.2.8
T6.3	Phase 6	Rollback Engine	T3.2.6
T6.4	Phase 6	Sequential/Parallel Logic	T6.1
T6.5	Phase 6	Retry Handler	T6.3
T6.6	Phase 6	Event Bus Integration	T6.2, T6.4
T7.1	Phase 7	Telemetry Logger	T6.6
T7.2	Phase 7	Quality Metrics Collector	T3.2.8
T7.3	Phase 7	Orchestration Map UI	T6.1
T7.4	Phase 7	Pattern Scoreboard UI	T5.8, T7.2
T7.5	Phase 7	Rating Interface	T7.4
T7.6	Phase 7	Feedback Pipeline	T7.5
T8.1	Phase 8	Failure Stubs	T3.2.3
T8.2	Phase 8	FS Event Recovery	T4.1
T8.3	Phase 8	Circuit Breaker	T6.5
T8.4	Phase 8	Rollback Testing	T6.3, T8.1


⸻

Let me know if you’d like a Notion-importable .csv or .md file download as well.