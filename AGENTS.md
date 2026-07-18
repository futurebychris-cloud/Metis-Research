Before making any changes, read the repository and especially AGENTS.md.

This project will be developed incrementally. Never build features that have not been requested.

For every future task, follow this workflow:

1. Read AGENTS.md.
2. Inspect the current repository.
3. Restate your understanding of the requested task in 3-6 bullet points.
4. Point out any ambiguity or architectural concern before writing code.
5. Only implement the requested scope.
6. Keep the implementation as small, clean, and maintainable as possible.
7. Avoid unnecessary abstractions and placeholder code.
8. Preserve existing behavior unless explicitly instructed otherwise.
9. Add or update tests only for the functionality introduced in the current task.
10. Run all relevant verification steps before declaring success.
11. Summarize exactly what changed, what was tested, and any remaining limitations.

Important project rules:

- This is a scientific research platform, not an AI trading bot.
- Never implement predefined trading strategies.
- Never leak future market information.
- Never implement features from future roadmap phases unless explicitly requested.
- Keep code beginner-readable with clear names, type hints, and comments where appropriate.
- If multiple implementation options exist, choose the one that maximizes long-term maintainability and explain why.

Architectural decision log:

- Maintain a running engineering and research history in `sizeable_changes.txt`.
- Add an entry whenever a significant design decision is made, such as a database choice, agent architecture, memory system, communication protocol, or replay assumption.
- Each entry must include the date, decision, reason it was chosen, alternatives considered, and potential drawbacks.
- Do not add entries for small implementation details.

For this message, DO NOT modify or create any files.

Instead, respond with:

1. Your understanding of the project's purpose.
2. Your understanding of the development philosophy.
3. The workflow you will follow for every future coding task.
4. Confirmation that you will build the project one chunk at a time and will not implement future features early.

Wait for the first implementation task before writing any code.
