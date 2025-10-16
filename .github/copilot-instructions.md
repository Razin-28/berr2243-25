## Repo overview

Small Node.js exercise project. Single entrypoint is `index.js`. The project uses the MongoDB Node driver (`mongodb` dependency in `package.json`). There is no build step — it's a simple script-run project.

## Big picture

- Purpose: demo/learning exercise showing a small in-memory `drivers` array and a simple MongoDB connect/insert/find flow in `index.js`.
- Components:
  - `index.js` — main script. Defines an example dataset (`drivers`) and an async `main()` which connects to MongoDB, inserts a document into `testDB.users`, queries it, and closes the client.
  - `package.json` — lists dependency `mongodb` (v6.x) and a placeholder `test` script.

## How to run and debug locally

- Requirements: Node.js (v18+ recommended) and a running MongoDB instance if you want `main()` to complete successfully. The script currently uses `mongodb://localhost:27017`.
- Run locally from the repository root:

  node index.js

- If MongoDB is not available, `main()` will throw an error when attempting to connect. You can still run the file to inspect the `drivers` array output which prints immediately.

## Project-specific patterns and conventions

- Single-file script style: this project favors top-level script with quick example logic (no separate modules). When extending, follow the pattern of exporting small functions and keeping `main()` as the process orchestrator.
- Minimal dependency management: only `mongodb` is declared. Prefer adding new dependencies to `package.json` and pin versions using caret ranges consistent with the existing file.
- Console-first debugging: `index.js` uses console logging liberally. New code should include clear console messages for connection status and DB operations.

## Integration points and external dependencies

- MongoDB connection string is hard-coded to `mongodb://localhost:27017`. If adding environment-aware behavior, follow this project's simple style: check `process.env.MONGODB_URI` and default to the current hard-coded value.
- Database and collection names are `testDB` and `users`. Tests or additional scripts that interact with the DB should either use their own DB/collection names or clearly reset/clean test data.

## Examples and snippets (from this repo)

- Connect/insert/query flow used in `index.js` (paraphrased):

  - create MongoClient with URI
  - client.connect()
  - const db = client.db('testDB')
  - const collection = db.collection('users')
  - collection.insertOne({ name: 'Razin', age: 22 })
  - collection.findOne({ name: 'Razin' })
  - client.close()

## What AI agents should prioritize

- Preserve the lightweight, example-first nature. Propose small, incremental changes (extract helper functions, add config via environment variables) rather than large refactors.
- When editing `index.js`, maintain the immediate `console.log(drivers)` behavior at top-level so the script remains informative when run without a DB.
- If adding tests or CI, ensure they do not require a local MongoDB unless explicitly marked as integration tests. Use mocks (e.g., `mongodb-memory-server` or mocking frameworks) for unit tests.

## Files to inspect first

- `index.js` — main source of truth for runtime behavior and examples
- `package.json` — dependencies and scripts

## Questions you can ask the repo owner (if uncertain)

- Should the MongoDB URI be configurable via env var? If yes, which env var name do you prefer? (suggest: MONGODB_URI)
- Do you want unit tests or CI added? If so, do you prefer in-memory DB tests or mocked drivers?

---
If anything in this file is unclear or you'd like more depth (examples for introducing config, tests, or refactors), tell me which part to expand.
