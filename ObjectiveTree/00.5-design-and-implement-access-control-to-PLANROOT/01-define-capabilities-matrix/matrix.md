# Capabilities / constraints matrix
Legend: **YES** (must be able to), **DC** (Don't Care), **NO** (should be impossible, or never attempted), **no** (should not be attempted directly: use `aap-*` instead.

## Mutations in `$PLANROOT`

| Operation                                          | Planner agent               | Coder agent                                   | User (planner session) | User (coder session) |
|----------------------------------------------------|-----------------------------|-----------------------------------------------|------------------------|----------------------|
| Run `aap-ls --fix`                                 | DC (`--fix` default)        | NO                                            | YES                    | DC                   |
| Create/rename/delete goal dirs in `ObjectiveTree/` | YES (when planning)         | NO                                            | YES                    | NO                   |
| Edit `status` files directly                       | no (use `aap-*`)            | no (use `aap-done`)                           | no (use `aap-*`)       | no (use `aap-done`)  |
| Edit `description` files                           | YES (planner work)          | NO                                            | DC (can use host)      | DC (discouraged)     |
| Edit other files in `current_objective`            | YES (planner work)          | NO                                            | DC (can use host)      | DC (can use host)    |
| Change `current_objective` directly                | no (use `aap-*`)            | no (use `aap-done`)                           | no (use `aap-*`)       | no (use `aap-done`)  |
| Run `aap-done <ref>`                               | NO (unless explicitly told) | YES                                           | YES                    | YES                  |
| Run `aap-previous`                                 | NO (unless explicitly told) | DC only when expl. authorized; otherwise NO   | YES                    | DC                   |
| Run `aap-insert …`                                 | DC (when planning)          | NO                                            | YES                    | DC (discouraged)     |

## Mutations in `$REPOROOT`

| Operation                                          | Planner agent               | Coder agent                                   | User |
|----------------------------------------------------|-----------------------------|-----------------------------------------------|------|
| Modify code / apply patches                        | NO                          | YES                                           | DC   |
| Run builds/tests                                   | DC (non-mutating checks ok) | YES                                           | DC   |

## Mounting / enforcement expectations

| Item                          | Requirement                                                                                                                     |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Primary enforcement           | Prefer OS-enforced RO/RW (mount/remount) over “policy text”, because coder+planner run as the same uid (`carlo`).               |
| “Read-only filesystem” errors | Treat as intentional. Agents NO attempt to bypass (no chmod/remount/debugging permissions unless the user explicitly requests). |
| Running `remountctl`          | User only. Agents NO. `aap-*` scripts DC run remountctl (using extra access control using the environment.                      |
