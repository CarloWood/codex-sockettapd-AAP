# Capabilities / constraints matrix
Legend: **MUST**, **MAY**, **MUST NOT**. “via `aap-*`” means no direct edits.

## Plan / progress in `$PLANROOT`

| Operation | Planner agent | Coder agent | User (planner session) | User (coder session) |
|---|---|---|---|---|
| Read `$PLANROOT/current_objective/description` | MUST | MUST | MAY | MAY |
| Run `aap-ls` (overview, `--fix`) | MAY (`--fix`) | MAY (`--no-fix` default) | MAY | MAY |
| Create/rename/delete goal dirs in `ObjectiveTree/` | MUST (when planning) | MUST NOT | MAY | MUST NOT |
| Edit `description` files | MUST (planner work) | MUST NOT | MAY | MUST NOT |
| Edit `status` files directly | MUST NOT (via `aap-*` only) | MUST NOT (via `aap-*` only) | SHOULD NOT (via `aap-*`) | SHOULD NOT (via `aap-*`) |
| Advance goal (`aap-done <ref>`) | MUST NOT (unless explicitly told) | MAY only when explicitly authorized; otherwise MUST NOT | MAY | MAY |
| Move backward (`aap-previous`) | MUST NOT (unless explicitly told) | MAY only when explicitly authorized; otherwise MUST NOT | MAY | MAY |
| Insert new goal (`aap-insert …`) | MAY (when planning) | MUST NOT | MAY | MUST NOT |
| Change `current_objective` directly | MUST NOT (via `aap-*` only) | MUST NOT (via `aap-*` only) | SHOULD NOT (via `aap-*`) | SHOULD NOT (via `aap-*`) |

## Code in `$REPOROOT`

| Operation | Planner agent | Coder agent | User |
|---|---|---|---|
| Read code / search (`findsymbol`, `gs`, `rg`) | MUST | MUST | MUST |
| Modify code / apply patches | MUST NOT | MUST | MAY |
| Run builds/tests | MAY (non-mutating checks ok) | MUST | MAY |

## Mounting / enforcement expectations

| Item | Requirement |
|---|---|
| Primary enforcement | Prefer OS-enforced RO/RW (mount/remount) over “policy text”, because coder+planner run as the same uid (`carlo`). |
| “Read-only filesystem” errors | Treat as intentional. Agents MUST NOT attempt to bypass (no chmod/remount/debugging permissions unless the user explicitly requests). |
| Running `remountctl` | User only. Agents MUST NOT. `aap-*` scripts MAY run remountctl (using extra access control using the environment. |
