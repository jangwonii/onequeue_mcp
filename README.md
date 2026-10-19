# OneQueue MCP

A local Model Context Protocol (MCP) server that helps AI agents inspect projects, run checks, verify browser behavior, track requirements, and assess release readiness. The connected AI agent writes and edits the application code.

This repository contains the MCP server, its tests, and configuration utilities. Example applications and hosted website sources are maintained separately and are not included here.

## Requirements and setup

Use Node.js 24 or later. The browser integration tests require Microsoft Edge.

```sh
npm ci
npm test
npm run client-config -- --write
```

Register the `command` and `args` from the generated `mcp-client.local.json` in your MCP host. Configure the host to launch the Node.js executable with the absolute path to `src/index.mjs`. Standard output is reserved for the MCP protocol; avoid using `npm start` as the host command because npm can add output.

## Project configuration

The default `projects.json` registers this MCP repository itself. To verify another project, set `root` to its directory and configure `sources` and `checks` for that project. Relative root paths are resolved from the configuration file's directory.

To use a separate configuration:

```sh
node src/index.mjs --config /absolute/path/projects.json
```

Preview support requires a `preview` entry specifying a server module, its exported function, and a `healthPath`. The default configuration does not define a preview server.

## Workflow

1. Call `inspect_project` to inspect the registered project and available checks.
2. Read `onequeue://guide` or select the `build_app` prompt for the development workflow.
3. Save requirements and acceptance criteria with `save_build_brief`.
4. Implement the code through your host agent, then collect evidence with `run_checks` and browser verification.
5. Use `get_project_status` to distinguish local verification, release readiness, and deployment verification.

Deployment is performed by the host's deployment tools. OneQueue manages verification evidence and release conditions. Source changes can invalidate previous evidence and require verification again.

## Read project status

```sh
npm run status -- --project onequeue
npm run status -- --project onequeue --full
```

Use `--full` to inspect detailed evidence. Do not start a second MCP server that owns the same project or forcibly delete its lock files.
