# CLAUDE.md

## Repository

Desired state for one MySQL failover cluster reachable at
`mysql-agy.bigconfig.online`: three DigitalOcean droplets in Amsterdam running MySQL
8.0 as a single-primary Group Replication group, with snapshots and continuously
archived binary logs in the `mysql-agy-backup` R2 bucket and a daily verified
restore. Behaviour lives in `../mysql-agy`, which depends only on `../green`.

Tracked source is `colors.yml`, the copied `green` launcher, the
`.agents/skills/package-mysql-agy-green/` payload and its `skills-lock.json`,
a secret-free `.envrc`, the toolchain files, and documentation. `.colors/` is
generated and `.envrc.private` is secret. Never read or commit either.

## Commands

```sh
./green build              # render only; no credential, no provider
./green create --dry-run   # walk the graph; touch nothing
./green create             # converge, snapshot, verify a restore, assert
./green health             # read-only assertions against the live cluster
./green delete             # guarded teardown
```

`build` and `--dry-run` need no credentials. A real create or delete requires
explicit authorization. Keep `compute-prevent-destroy: true`; an authorized
delete uses a one-run `COLORS_PAR_COMPUTE_PREVENT_DESTROY=false` and never an
edit to the file. Never set `COLORS_PAR_PROFILE`.

## Coupling

The root launcher is a **copy** of `.agents/skills/package-mysql-agy-green/green`,
not a symlink. `npx skills update -p` rewrites the payload and leaves the root
file alone, so after every package update re-copy it and check they are
byte-identical:

```sh
npx skills update -p
cp .agents/skills/package-mysql-agy-green/green green
cmp green .agents/skills/package-mysql-agy-green/green
```

During development use `MYSQL_AGY_LIB_ROOT=../mysql-agy`; a final run must use the
real pushed SHA in both copies.

## Git

Work on the current branch. Do not commit or push unless explicitly authorized.
