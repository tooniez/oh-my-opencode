# AGENTS KNOWLEDGE BASE

## OVERVIEW
AI agent definitions for multi-model orchestration, delegating tasks to specialized experts.

## STRUCTURE
```
agents/
├── orchestrator-sisyphus.ts # Orchestrator agent (1485 lines) - 7-section delegation, wisdom
├── sisyphus.ts              # Main Sisyphus prompt (643 lines)
├── sisyphus-junior.ts       # Junior variant for delegated tasks
├── oracle.ts                # Strategic advisor (GPT-5.2)
├── librarian.ts             # Multi-repo research (GLM-4.7-free)
├── explore.ts               # Fast codebase grep (Grok Code)
├── frontend-ui-ux-engineer.ts  # UI generation (Gemini 3 Pro Preview)
├── document-writer.ts       # Technical docs (Gemini 3 Pro Preview)
├── multimodal-looker.ts     # PDF/image analysis (Gemini 3 Flash)
├── prometheus-prompt.ts     # Planning agent prompt (991 lines) - interview mode
├── metis.ts                 # Plan Consultant agent - pre-planning analysis
├── momus.ts                 # Plan Reviewer agent - plan validation
├── build-prompt.ts          # Shared build agent prompt
├── plan-prompt.ts           # Shared plan agent prompt
├── sisyphus-prompt-builder.ts # Factory for orchestrator prompts
├── types.ts                 # AgentModelConfig interface
├── utils.ts                 # createBuiltinAgents(), getAgentName()
└── index.ts                 # builtinAgents export
```

## AGENT MODELS
| Agent | Default Model | Purpose |
|-------|---------------|---------|
| Sisyphus | openrouter/deepseek/deepseek-v3:free | Primary orchestrator. DeepSeek V3 for complex orchestration tasks. |
| oracle | openrouter/deepseek/deepseek-r1-0528:free | High-IQ debugging, architecture, strategic consultation. DeepSeek R1 for best reasoning. |
| librarian | openrouter/google/gemini-2.0-flash-exp:free | Multi-repo analysis, docs research, GitHub examples. Gemini 2.0 Flash with 1M context window. |
| explore | openrouter/xiaomi/mimo-v2-flash:free | Fast contextual grep. MiMo-V2-Flash (#1 open model on SWE-Bench) for coding tasks. |
| frontend-ui-ux | openrouter/google/gemini-2.0-flash-exp:free | Production-grade UI/UX generation and styling. Gemini 2.0 Flash with multimodal support. |
| document-writer | openrouter/google/gemini-2.0-flash-exp:free | Technical writing, guides, API documentation. Gemini 2.0 Flash with 1M context. |
| multimodal-looker | openrouter/google/gemini-2.0-flash-exp:free | PDF/image analysis. Gemini 2.0 Flash with 1M context and multimodal vision-language capabilities. |
| Prometheus | openrouter/deepseek/deepseek-r1-0528:free | Strategic planner. Interview mode, orchestrates Metis/Momus. DeepSeek R1 for reasoning. |
| Metis | openrouter/deepseek/deepseek-r1-0528:free | Plan Consultant. Pre-planning risk/requirement analysis. DeepSeek R1 for reasoning. |
| Momus | openrouter/deepseek/deepseek-r1-0528:free | Plan Reviewer. Validation and quality enforcement. DeepSeek R1 for reasoning. |
| orchestrator-sisyphus | openrouter/deepseek/deepseek-v3:free | Orchestrates work via delegate_task() to complete ALL tasks in a todo list. DeepSeek V3 for general purpose. |

## HOW TO ADD AN AGENT
1. Create `src/agents/my-agent.ts` exporting `AgentConfig`.
2. Add to `builtinAgents` in `src/agents/index.ts`.
3. Update `types.ts` if adding new config interfaces.

## MODEL FALLBACK LOGIC
`createBuiltinAgents()` handles resolution:
1. User config override (`agents.{name}.model`).
2. Environment-specific settings (max20, antigravity).
3. Hardcoded defaults in `index.ts`.

## ANTI-PATTERNS
- **Trusting reports**: NEVER trust subagent self-reports; always verify outputs.
- **High temp**: Don't use >0.3 for code agents (Sisyphus/Prometheus use 0.1).
- **Sequential calls**: Prefer `delegate_task` with `run_in_background` for parallelism.

## SHARED PROMPTS
- **build-prompt.ts**: Unified base for Sisyphus and Builder variants.
- **plan-prompt.ts**: Core planning logic shared across planning agents.
- **orchestrator-sisyphus.ts**: Uses a 7-section prompt structure and "wisdom notepad" to preserve learnings across turns.
