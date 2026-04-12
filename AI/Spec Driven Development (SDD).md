#TODO 
The practice of rigorously *defining the exact behavior, constraints, and evaluation criteria* of an AI system *before* you start writing prompts, fine-tuning models, or building agents.

It's designed to handle the unpredictable nature of LLMs.
## Core Workflow
 Typically follows a structured multi-phased lifecycle to minimize unpredictable responses, although most implementations have different exact steps.
 1. **Specify**: Define *functional requirements*, *user stories*, and measurable *success criteria*
 2. **Plan**: Translate requirements into *technical architecture*, making explicit decisions about frameworks, database schemas, and integration points
 3. **Task**: Decompose the plan into atomic, testable *units of work* that an AI agent can execute in isolation  
 4. **Implement**: Use AI agents to generate code based on specifications. Can use a "verifier agent" or CI/CD pipeline to validate code against original spec to *prevent "architectural drift"*
## Benefits vs. Challenges
**Benefits**:
- *Reduces hallucinations* using strong "context anchoring"
- Enforces exhaustive and up-to-date *documentation*
- Allows finer review of the architecture and business logic over nitpicking syntax in a PR, shifting the review process "left"

**Challenges**:
- *High upfront investment costs* and can lead to overwhelming amounts of *subjective prompts* over objective code
- *"Waterfall-style" trap* of rigidity if the spec is not treated correctly. It should be treated as mutable and as a quick draft. Taking too long on the spec leads to doing Waterfall.
## Greenfield vs. Brownfield
The way you use SDD depends on whether you use it for greenfielding or brownfielding.

**Greenfielding** is often easier since you have 0 initial context to manage. AI is very quick to suggest refactors and changes to existing systems, so letting it start from scratch ensures that it already keeps track of all the design choices and can note edge cases along the way.

**Brownfielding** is considered harder because the "spec" is often missing or outdated. Applying the standard greenfielding approach would likely lead to the AI rewriting or naively refactoring large pieces of legacy code, not being aware of hidden or complex dependencies. In order you adapt it for brownfielding, you should:
1. *Context Extraction*: Use tools like ==TODO== to map the dependency graph of the relevant module
2. *Baseline Spec*: Ask the AI to create a formal spec describing the *current* behavior of a module
3. *Delta Spec*: Edit the generated spec to reflect your desired changes. Now the AI is aware of the "before" and "after" context, reducing the risk of breaking existing logic

When performing brownfielding, it's recommended to use a *"spec-anchored"* tool (like Intent or OpenSpec) that maintains a "living spec", helping keep a memory of changes over time and remain consistent.

## Popular Tools
- OpenSpec