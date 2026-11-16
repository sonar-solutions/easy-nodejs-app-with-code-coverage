# CLAUDE.md

Small Express API (customer CRUD) with AvaJS tests and c8 coverage. It is a SonarQube Cloud demo project.

- SonarQube Cloud project: `sonar-solutions_easy-nodejs-app-with-code-coverage` (org `sonar-solutions`), linked in `.sonar-config.json`.
- `risky_code/` holds deliberately vulnerable or smelly samples. Each file name matches the Sonar rule it triggers, for example `javascript_S5547.js`. They exist so Sonar has findings to show. Do not fix, refactor or delete them unless asked.
- Commands: `npm install`, `npm test` (runs `c8 ava`, writes `coverage/lcov.info`), `npm start` (port 9020).

## Vortex is required in this repo

Use Sonar Vortex (via the `sonar` CLI, version 1.11.0 or newer) for every task here. If `sonar` is missing or not logged in, say so once and point to the setup steps below. Do not silently skip it.

### 1. Before reading code: use Vortex context

Prefer these over grepping or reading whole files when you need to understand the code:

```bash
sonar context navigation get-map                                   # architecture: top-level blocks and dependencies
sonar context navigation search-signatures --pattern <text>        # find declarations and their exact names (FQNs)
sonar context navigation search-bodies --pattern <text>            # find call sites inside function bodies
sonar context navigation get-source --fqn <fqn>                    # read one symbol
sonar context navigation trace-callers --fqn <fqn>                 # who calls it
```

### 2. Before writing or editing source code: load the guidelines

```bash
sonar context guidelines get
```

Follow the rules it returns in all code you write. Before changing `package.json` or `package-lock.json`, check each new or changed package:

```bash
sonar context dependencies check --purl pkg:npm/<name>@<version>
```

### 3. Before you reply: run Vortex analysis on what you changed

At the end of any turn that changed files, analyze all of them in one command, from the repo root, with DEEP depth:

```bash
sonar analyze agentic --depth DEEP --file <path1> --file <path2>
# or, if you cannot list every changed path:
sonar analyze agentic --depth DEEP
```

- Fix findings on lines you changed, then re-run the same command until none remain.
- Leave findings on lines you did not touch, including everything in `risky_code/`, unless asked.
- Paste any remaining findings verbatim in your reply.
- Files Sonar does not analyze (such as Markdown) are skipped. That is expected and needs no mention.

## Setup, if Vortex is not working

```bash
sonar auth login -s https://sonarcloud.io -o sonar-solutions
sonar integrate claude          # choose "Install recommended"
sonar auth status               # should show Connected, with no "different connection" warning
```

Then restart Claude Code. The SonarQube MCP server that `sonar integrate` installs runs in a container, so Docker Desktop (or another container runtime) must be available. The `sonar context` and `sonar analyze agentic` commands above do not need Docker.
