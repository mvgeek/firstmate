# Live lab: durable Claude config store for a secondmate

Lab: `bin/fm-live-lab.sh up --harness claude --mate --supervision-host none /tmp/fmlab.eN3IEA`
(target commit 93b09540, real tmux on a private socket, real `claude` CLI, real lab primary answered probe).
Operator shell ambient store: `CLAUDE_CONFIG_DIR=/Users/aman/.claude-personal`.
Alternate stores: `/tmp/nm-01M4-stores/store-work` (relauncher ambient), `/tmp/nm-01M4-stores/store-other` (override).
"claude env" = CLAUDE_CONFIG_DIR read from `ps eww` of the live claude process in the mate's pane.

| step | command (lab env) | record claude_config_dir= | claude env |
|---|---|---|---|
| 1 initial spawn | `fm-spawn.sh <mate> --secondmate` (ambient ~/.claude-personal) | /Users/aman/.claude-personal | /Users/aman/.claude-personal (pid 58282) |
| 2 control relaunch | `CLAUDE_CONFIG_DIR=store-work fm-control.sh <mate> relaunch` | /Users/aman/.claude-personal | /Users/aman/.claude-personal (new pid 90513) |
| 3 crash + session-start sweep | kill claude; `CLAUDE_CONFIG_DIR=store-work fm-bootstrap.sh` (ledger: attempt, relaunched) | /Users/aman/.claude-personal | /Users/aman/.claude-personal (new pane, pid 645) |
| 4 deliberate override | `CLAUDE_CONFIG_DIR=store-work FM_CLAUDE_CONFIG_DIR=store-other fm-control.sh <mate> relaunch` | store-other | store-other (pid 24609; pane shows fresh-store onboarding theme picker) |
| 5 recorded store vanished | mv store-other away; `fm-control.sh <mate> relaunch` | unchanged (meta byte-identical) | still pid 24609 on store-other: refused before stop |
| 6 crash, store still gone | kill claude; `fm-spawn.sh <mate> --relaunch` | unchanged | none launched: refused |
| 7 store restored | `CLAUDE_CONFIG_DIR=store-work fm-spawn.sh <mate> --relaunch` | store-other | store-other (pid 34714) |

Refusal text (steps 5 and 6):
error: the task's recorded Claude config store '/tmp/nm-01M4-stores/store-other' (claude_config_dir= in .../state/lab75a29431d9f5-mate.meta) no longer exists; refusing rather than launching on a different account - restore that directory or relaunch with FM_CLAUDE_CONFIG_DIR=<store> (or 'default') to choose one deliberately

The relauncher's ambient store-work stayed empty throughout (0 entries): nothing ever launched or registered trust there.
Teardown: `bin/fm-live-lab.sh down` stopped the lab server, removed 2 lab trust entries from ~/.claude-personal, removed the lab root.
