#TODO 
The practice of rigorously *defining the exact behavior, constraints, and evaluation criteria* of an AI system *before* you start writing prompts, fine-tuning models, or building agents.

It's designed to handle the unpredictable nature of LLMs.

==TODO==: Talk about green-fielding vs brown-fielding
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
## Popular Tools
