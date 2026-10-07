# Live lab transcript: per-project no-mistakes home routing (real bin/fm-spawn.sh, real tmux socket fm-lab, real treehouse pool, real claude auth via claude-p/claude-w wrappers)
## config/no-mistakes-homes
bookie $LAB/nm-personal
vibrantly $LAB/nm-work
## nm-personal/config.yaml
agent: claude
agent_path_override:
  claude: $LAB/bin/claude-p
## nm-work/config.yaml
agent: claude
agent_path_override:
  claude: $LAB/bin/claude-w
## spawn nm-bookie
worker probe:
NM_HOME=$LAB/nm-personal
PWD=$LAB/pool/.treehouse/bookie-ebbb5e/1/bookie
remote=$LAB/nm-personal/repos/bookie-id.git
task record: present
## spawn nm-vibrantly
spawned nm-vibrantly harness=sh kind=ship mode=no-mistakes yolo=off window=ctl:fm-nm-vibrantly worktree=$LAB/pool/.treehouse/vibrantly-b80c87/1/vibrantly
SPAWN_EXIT=0
worker probe:
NM_HOME=$LAB/nm-work
PWD=$LAB/pool/.treehouse/vibrantly-b80c87/1/vibrantly
remote=$LAB/nm-work/repos/vibrantly-id.git
task record: present
## spawn nm-scratch
spawned nm-scratch harness=sh kind=ship mode=no-mistakes yolo=off window=ctl:fm-nm-scratch worktree=$LAB/pool/.treehouse/scratch-78ad93/1/scratch
SPAWN_EXIT=0
worker probe:
NM_HOME=unset
PWD=$LAB/pool/.treehouse/scratch-78ad93/1/scratch
remote=
task record: present
## spawn dp-bookie
notice: dp-bookie ships mode=direct-PR while the standing posture for bookie is no-mistakes - less rigor than the captain's standing posture; proceed only on a current explicit captain instruction or an intake judgment you can state
spawned dp-bookie harness=sh kind=ship mode=direct-PR yolo=off window=ctl:fm-dp-bookie worktree=$LAB/pool/.treehouse/bookie-ebbb5e/2/bookie
SPAWN_EXIT=0
worker probe:
NM_HOME=unset
PWD=$LAB/pool/.treehouse/bookie-ebbb5e/2/bookie
remote=$LAB/nm-personal/repos/bookie-id.git
task record: present
## spawn so-bookie
error: config/no-mistakes-homes routes bookie to $LAB/nm-personal, whose Claude account wrapper $LAB/bin/claude-signedout is not signed in ($LAB/bin/claude-signedout auth status); sign that account in, or change the mapping
SPAWN_EXIT=1
task record: absent
## spawn iso-vibrantly
spawned iso-vibrantly harness=sh kind=ship mode=no-mistakes yolo=off window=ctl:fm-iso-vibrantly worktree=$LAB/pool/.treehouse/vibrantly-b80c87/2/vibrantly
SPAWN_EXIT=0
worker probe:
NM_HOME=$LAB/nm-work
PWD=$LAB/pool/.treehouse/vibrantly-b80c87/2/vibrantly
remote=$LAB/nm-work/repos/vibrantly-id.git
task record: present
## spawn gm-bookie
error: config/no-mistakes-homes routes bookie to $LAB/nm-personal, but its gate is registered under $LAB/nm-work; move the gate only when no run is active (git -C /var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T/fm-lab.o3fCI9/projects/bookie remote remove no-mistakes, then NM_HOME=$LAB/nm-personal no-mistakes init), or change the mapping
SPAWN_EXIT=1
task record: absent
## spawn raw-bookie2
error: config/no-mistakes-homes routes bookie to $LAB/nm-personal, but the raw launch command sets NM_HOME, which would override that route; remove it from the raw command
SPAWN_EXIT=1
task record: absent
## spawn raw-scratch
spawned raw-scratch harness=sh kind=ship mode=no-mistakes yolo=off window=ctl:fm-raw-scratch worktree=$LAB/pool/.treehouse/scratch-78ad93/2/scratch
SPAWN_EXIT=0
worker probe:
NM_HOME=$LAB/scratch-nm
PWD=$LAB/pool/.treehouse/scratch-78ad93/2/scratch
remote=
task record: present
## spawn mf-bookie
error: config/no-mistakes-homes line 3 must be <project> <absolute NM_HOME>: $LAB/config/no-mistakes-homes
SPAWN_EXIT=1
task record: absent
## lab tmux windows
ctl:bash
ctl:fm-nm-bookie
ctl:fm-nm-vibrantly
ctl:fm-nm-scratch
ctl:fm-dp-bookie
ctl:fm-iso-vibrantly
ctl:fm-raw-scratch
## default daemon after
  ● daemon running (pid 92063)
## spawn nm-bookie (log)
spawned nm-bookie harness=sh kind=ship mode=no-mistakes yolo=off window=ctl:fm-nm-bookie worktree=$LAB/pool/.treehouse/bookie-ebbb5e/1/bookie
SPAWN_EXIT=0
worker probe:
NM_HOME=$LAB/nm-personal
PWD=$LAB/pool/.treehouse/bookie-ebbb5e/1/bookie
remote=$LAB/nm-personal/repos/bookie-id.git
task record: present
## gate backstop: fm-spawn from worktree of $LAB/nm-personal/repos/bookie-id.git, NO_MISTAKES_GATE unset, unmarked FM_HOME
error: refusing fleet lifecycle from inside a no-mistakes gate worktree ($LAB/nm-personal/repos/bookie-id.git)
exit=3
## look-alike $LAB/look/projects/repos/x.git (no state.sqlite): not treated as gate
error: task bs-2 has no brief at inaccessible data path $LAB/notlab/data/bs-2/brief.md
