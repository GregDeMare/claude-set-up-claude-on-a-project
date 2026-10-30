Verified /memory shows CLAUDE.md loaded and /permissions shows my rules

Overview of Changes
I have optimized and structured the CLAUDE.md file to act as a high-signal configuration guide for Claude Code, specifically tailored to this project's stack (Express.js, CommonJS, and Node's native test runner).
Key Adjustments Made:
• Transitioned to "Use X, not Y" format: Rewrote the guidelines into clear, actionable, and explicit constraints. This format prevents LLM optimization loops and gives the model concrete destination boundaries.
• Eliminated Redundancies: Removed duplicate structural rules (such as repeating that route handlers shouldn't touch data in both the convention and architecture sections).
• Removed Configuration Fluff: Stripped out verbose "why" explanations (e.g., references to ESLint legacy versions or missing Jest frameworks) because Claude reads package.json and config files automatically upon initialization.
• Streamlined Commands: Condensed the command list to essential entry points (dev, test, lint, inspect), ensuring maximum token efficiency while keeping the instructions highly actionable.
• Preserved Critical Architectural Guards: Maintained and sharpened instructions regarding the conditional listen() block in server.js (require.main === module) and the shared-state nature of db/store.js to prevent broken test generation.
• I added to conventions: use central error handling, not inline try catch
• I added allow npm test, ask git push, deny read .env ./.env & git push --force
• If git --force is allowed dataloss is possible, protects main/master branches, preserve traceability

Key Adjustments Made to .claude/settings.json:
• Enforced Destructive Command Safety (Git Protection): Added explicit rules to deny or prevent destructive terminal actions like git push --force or git push -f. This serves as an essential guardrail against LLM "hallucination loops", where a model might try to force-push to resolve a git conflict, risking permanent remote data loss.
• Secured Environment Files (.env): Restricted Claude's ability to read or modify the local .env file containing application secrets, forcing the model to strictly interact with .env.example instead.
• Isolated Project Scope: Transitioned permissions to a localized configuration structure to ensure that automated actions (like automated testing or linting via npm test and npm run lint) are tightly bound to this project repository's boundaries, protecting global system files.
