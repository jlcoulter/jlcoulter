# Projects

A big list of things to pick up and do. Complexity rated 1 (trivial) to 5 (hard).

---

## Existing repos — improvements

### 1. Basiq integration in financials  [complexity: 4/5]
- Create `crates/basiq` in `~/git/financials`, use reqwest to call the Basiq API
- Store transactions in a new `transactions` table via sqlx
- Add axum route `GET /api/transactions`
- Put `BASIQ_API_KEY` in `.env`

### 2. Publish kbauto-placeholder to crates.io  [complexity: 1/5]
- Standalone crate, no internal deps — just publish it

### 3. nixpkgs PR for kbauto  [complexity: 3/5]
- Use nix-init to generate the derivation
- Bump in nixpkgs, `nix build`, open PR

### 4. Everforest theme flake-parts module  [complexity: 4/5]
- Extract palette + Kitty/Waybar/GTK/Zellij injection from flake into a standalone flake-parts module
- Publish as `everforest-nix` on GitHub

### 5. CSV export endpoint in financials  [complexity: 2/5]
- `GET /api/transactions/export?format=csv` using the csv crate, streamed
- Add a download button to the frontend

### 6. nvf bump PR in nixpkgs  [complexity: 2/5]
- Fork NixOS/nixpkgs, bump `pkgs/by-name/nv/nvf/default.nix`
- `nix build`, open PR

### 7. CONTRIBUTING.md + issue templates for kbauto  [complexity: 1/5]
- Add `CONTRIBUTING.md`
- Set up bug report and feature request templates via `gh issue create --template`

### 8. nvim-config Lua module restructure  [complexity: 3/5]
- Replace `init.lua` with `init.lua` + `lua/options.lua` + `lua/keymaps.lua` + `lua/plugins/` (one file per plugin)

~~### 9. CI for kbauto  [complexity: 1/5]~~ ✅
- ~~`.github/workflows/ci.yml` running `cargo test --all` + `cargo clippy` on push~~
- ~~Add passing badge to README~~

~~### 10. CI for financials  [complexity: 1/5]~~ ✅
- ~~Same pattern — `cargo test`, `cargo clippy`, `cargo fmt --check`~~
- ~~Add the badge~~

### 11. CHANGELOG.md for kbauto  [complexity: 1/5]
- keepachangelog format, document the initial release features

### 12. docker-compose.yml for pageturner  [complexity: 2/5]
- Verify the existing compose actually builds and runs
- Add a "Deploy" section to README with one-command start

---

## New CLI tools — Rust

### envcheck — validate .env files against code  [complexity: 4/5]
- Scan Rust/Go/JS codebase for env var references, compare against `.env` and `.env.example`
- Report missing, unused, undocumented variables
- Uses tree-sitter, clap, walkdir

### dockviz — docker compose dependency visualizer  [complexity: 3/5]
- Read `docker-compose.yml`, output visual graph of service deps (depends_on, links, networks)
- ASCII or SVG output
- Uses serde_yaml, graph rendering, crossterm

### cronlint — validate and explain cron expressions  [complexity: 2/5]
- `cronlint "0 2 * * 1-5"` → "Runs at 02:00 on Monday through Friday"
- Validate ranges, step values, flag impossible schedules
- Lint crontab files for overlapping jobs
- Pure logic, pattern matching, enum errors

### flake-trace — Nix flake dependency analyzer  [complexity: 3/5]
- Read `flake.lock`, output tree of transitive deps with versions + last-updated dates
- Flag outdated inputs
- Uses serde_json, fills a real gap in the Nix ecosystem

### logdelta — diff structured logs  [complexity: 3/5]
- Take two JSON log files, show what changed between runs
- Compare levels, fields, timing — think jq but for comparing, not filtering
- Uses serde_json, zero-copy parsing

### secretsweep — scan repos for committed secrets  [complexity: 4/5]
- Like gitleaks but faster (Rust), focus on Nix flakes, .env, YAML configs
- Scan git history, not just working tree
- Pre-commit hook integration
- Uses regex performance + git traversal speed

### marksplit — split markdown by headings  [complexity: 2/5]
- `marksplit --level 2 README.md` → one file per `##` section
- Uses pulldown-cmark, string handling

### schemer — database schema diff + migration generator  [complexity: 5/5]
- Connect to two DBs (or read SQL files), diff schema, generate migration SQL
- `schemer diff --from dev.db --to prod.db` outputs ALTER TABLE statements
- Uses sqlx + diesel schema introspection

### treewatch — filesystem watcher with command triggers  [complexity: 3/5]
- Watch a dir tree, run commands when files change
- Compound triggers (run A on .rs change, B on .toml change, C after debounce)
- Uses notify crate

### jless successor — interactive JSON explorer  [complexity: 4/5]
- TUI for navigating large JSON files with fuzzy search, path copying, JSONPath queries
- Uses Ratatui + serde_json + crossterm

### cargo-outdated-check — detect EOL/yanked deps  [complexity: 3/5]
- Read Cargo.lock, check each crate against crates.io API
- Report yanked, security advisories (rustsec), or stale (2+ years) crates
- `--fix` opens PRs

### nixfmt-tree — format and organize Nix flake outputs  [complexity: 4/5]
- Parse `flake.nix`, sort outputs alphabetically, align attribute sets, format with nixpkgs-fmt
- Report unused inputs, `--check` for CI
- Uses rnix parser

### reqbench — HTTP benchmarking with structured output  [complexity: 3/5]
- Send N concurrent requests, report p50/p90/p99 latency, throughput, error rate
- JSON output for CI comparison
- Uses reqwest + tokio + hdrhistogram

### tomlfmt — opinionated TOML formatter  [complexity: 3/5]
- Consistent key ordering, array styling, comment alignment
- `tomlfmt Cargo.toml --check` for CI
- Uses toml_edit (preserves comments while restructuring)

### diffstaged — smart git diff for staged changes  [complexity: 4/5]
- TUI showing staged hunks with syntax highlighting + type-aware change detection
- Stage individual lines from the TUI
- Uses similar + syntect + ratatui

### lockfile-diff — dependency change analyzer  [complexity: 3/5]
- Diff two Cargo.lock / go.sum / package-lock.json files
- Show version changes with links to changelogs
- Drop in CI to annotate PRs with dependency changes
- Works across ecosystems

### configtest — validate NixOS configs before applying  [complexity: 4/5]
- Parse NixOS config, check for conflicting options, deprecated settings, missing imports, duplicate packages
- `configtest flake.nix --host default` runs without building
- Uses rnix parser + rule engine

---

## New CLI tools — Go

### dbmux — database connection switcher  [complexity: 3/5]
- Manage multiple DB connections (dev, staging, prod) across projects
- Store configs, switch active connection per project
- Go exec/subprocess glue + config management

### portfold — port management for local dev  [complexity: 2/5]
- Track which ports projects use, detect conflicts on new services
- `portfold kill 8080` to kill processes by port
- `.portfold.yml` per project declaring expected ports
- Replaces the `lsof -i :8080` dance

### gitignore-gen — language-aware .gitignore generator  [complexity: 2/5]
- Detect languages in repo (file extensions, Cargo.toml, go.mod, package.json)
- Generate merged .gitignore — additive and project-specific
- `--check` audits existing .gitignore for missing patterns

### healthpinger — distributed health check monitor  [complexity: 3/5]
- Single binary, pings endpoints from YAML config, sends Slack/Discord alerts on downtime
- State in SQLite, `healthpinger start` / `healthpinger status`
- Self-hosted UptimeRobot in ~500 lines

### tunneler — local tunnel alternative to ngrok  [complexity: 4/5]
- Expose local port via reverse SSH tunnel to a public server
- `tunneler 8080` gives a public URL
- Server side is a single binary for your VPS

### webhookd — lightweight webhook receiver  [complexity: 3/5]
- Listen for incoming webhooks, validate signatures, run configured shell commands or forward
- Config via YAML, `webhookd --port 9000 --config hooks.yml`
- Uses net/http + crypto/hmac natively

### migrater — cross-engine database migration tool  [complexity: 4/5]
- Run migrations for PostgreSQL, MySQL, SQLite with one binary
- `migrater up` / `migrater down` / `migrater create "add users table"`
- Uses database/sql + drivers for all three

### proxyrate — rate-limiting reverse proxy  [complexity: 3/5]
- Enforce rate limits per IP/token/route in front of an API
- `proxyrate --config limits.yml --upstream http://localhost:8080`
- Uses reverse proxy + time/rate packages

### logtail — aggregate and filter logs from multiple sources  [complexity: 3/5]
- Tail log files and/or Docker containers, apply filters, forward to stdout or webhook
- `logtail --file /var/log/app.log --container api --filter "ERROR"`
- Think `tail -f` but smarter and across sources

### backupr — simple encrypted backup to S3-compatible storage  [complexity: 4/5]
- Tar a directory, encrypt with age/gpg, upload to S3/R2/Minio
- `backupr /home/jc/documents --dest s3://my-bucket --encrypt`
- Cron it. Uses tar + crypto + aws-sdk-go-v2

### confval — validate config files against schemas  [complexity: 3/5]
- `confval config.yml --schema schema.json` reports errors with line numbers
- Supports JSON Schema + custom rules
- Go encoding packages + structured error reporting

### portfwd — SSH port forwarding manager  [complexity: 3/5]
- Manage persistent SSH tunnels to multiple servers
- `portfwd up` / `portfwd down` / `portfwd status`, auto-reconnect on failure
- Replaces the "10 SSH tunnels in tmux" problem

### apidiff — compare API responses over time  [complexity: 3/5]
- Record API responses, compare across runs, report breaking changes
- `apidiff record --url ...` then `apidiff compare`
- Uses encoding/json + reflect for structural comparison