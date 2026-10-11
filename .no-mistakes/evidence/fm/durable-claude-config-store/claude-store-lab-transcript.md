# Live lab transcript: durable Claude config store (fm-spawn / fm-control against real tmux + real claude 2.1.296)
Lab FM_HOME=/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T//fm-lab.45N9WT (marked lab home, private tmux socket fm-lab); HOME=/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T//fm-lab.45N9WT/userhome so Claude's default store is disposable.
Stores: store-work and store-personal are empty disposable dirs (no login); claude reaches composer as 'Not logged in'.

## S1 first secondmate spawn, ambient CLAUDE_CONFIG_DIR=store-work
$ rc=0
warning: secondmate sm1 sync skipped before launch: primary default-branch commit cannot be resolved
spawned sm1 harness=claude kind=secondmate mode=secondmate yolo=off window=primary:fm-sm1 worktree=/private/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T/fm-labsm.tXWruU/smhome

## S2 secondmate dies (window killed), liveness-style respawn 'fm-spawn sm1 --secondmate' from ambient store-personal
$ rc=0
warning: secondmate sm1 sync skipped before launch: primary default-branch commit cannot be resolved
spawned sm1 harness=claude kind=secondmate mode=secondmate yolo=off window=primary:fm-sm1 worktree=/private/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T/fm-labsm.tXWruU/smhome

## S3 'fm-control sm1 relaunch' from ambient store-personal
$ rc=0
warning: secondmate sm1 sync skipped before launch: primary default-branch commit cannot be resolved
relaunched sm1 harness=claude from=claude model=default effort=default backend=tmux endpoint=primary:fm-sm1 worktree=/private/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T/fm-labsm.tXWruU/smhome

## S5 adversarial: store-work moved away, 'fm-control sm1 relaunch' from ambient store-personal
$ rc=1
error: the task's recorded Claude config store '/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T//fm-lab.45N9WT/store-work' (claude_config_dir= in /var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T//fm-lab.45N9WT/state/sm1.meta) no longer exists; refusing rather than launching on a different account - restore that directory or relaunch with FM_CLAUDE_CONFIG_DIR=<store> (or 'default') to choose one deliberately

## S4 'FM_CLAUDE_CONFIG_DIR=default fm-control sm1 relaunch' from ambient store-personal
$ rc=0
warning: secondmate sm1 sync skipped before launch: primary default-branch commit cannot be resolved
relaunched sm1 harness=claude from=claude model=default effort=default backend=tmux endpoint=primary:fm-sm1 worktree=/private/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T/fm-labsm.tXWruU/smhome

## S4b secondmate dies, respawn from ambient store-personal after default marker
$ rc=0
warning: secondmate sm1 sync skipped before launch: primary default-branch commit cannot be resolved
spawned sm1 harness=claude kind=secondmate mode=secondmate yolo=off window=primary:fm-sm1 worktree=/private/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T/fm-labsm.tXWruU/smhome

## S6 scout worker spawn, ambient store-work
$ rc=0
spawned sc1 harness=claude kind=scout window=primary:fm-sc1 worktree=/private/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T/fm-lab.45N9WT/userhome/.treehouse/demo-c39628/1/demo

## S6b 'fm-control sc1 relaunch' from ambient store-personal
$ rc=0
relaunched sc1 harness=claude from=claude model=default effort=default backend=tmux endpoint=primary:fm-sc1 worktree=/private/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T/fm-lab.45N9WT/userhome/.treehouse/demo-c39628/1/demo

## S7 fresh scout spawn, ambient store-personal, tmux server env CLAUDE_CODE_USE_BEDROCK=1
$ rc=0
spawned sc2 harness=claude kind=scout window=primary:fm-sc2 worktree=/private/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T/fm-lab.45N9WT/userhome/.treehouse/demo-c39628/2/demo

## Observed claude process environment (ps eww on the pane's claude pid) and task record after each step
S1  sm1 meta claude_config_dir=<lab>/store-work ; claude pid 43084 CLAUDE_CONFIG_DIR=<lab>/store-work ; trust registered in store-work/.claude.json projects
S2  sm1 meta claude_config_dir=<lab>/store-work ; claude pid 52660 CLAUDE_CONFIG_DIR=<lab>/store-work ; store-personal untouched (empty)
S3  sm1 meta claude_config_dir=<lab>/store-work ; new claude pid 77665 CLAUDE_CONFIG_DIR=<lab>/store-work
S5  rc=1, refusal names missing store; meta byte-identical (cmp); old claude pid 77665 still running on store-work
S4  sm1 meta claude_config_dir=default ; claude pid 83282 has NO CLAUDE_CONFIG_DIR in env
S4b sm1 meta claude_config_dir=default ; claude pid 87891 has NO CLAUDE_CONFIG_DIR in env ; trust registered in $HOME/.claude.json (default store)
S6  sc1 meta claude_config_dir=<lab>/store-work ; claude pid 8982 CLAUDE_CONFIG_DIR=<lab>/store-work
S6b sc1 meta claude_config_dir=<lab>/store-work ; new claude pid 16167 CLAUDE_CONFIG_DIR=<lab>/store-work
S7  sc2 meta claude_config_dir=<lab>/store-personal (no claude_config_pinned) ; claude pid 27019 CLAUDE_CONFIG_DIR=<lab>/store-personal CLAUDE_CODE_USE_BEDROCK=1 (provider selector kept)
pid=87891 comm=claude
HOME=/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T//fm-lab.45N9WT/userhome
CLAUDE_CONFIG_DIR: (absent from process env)
pid=16167 comm=claude
HOME=/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T//fm-lab.45N9WT/userhome
CLAUDE_CONFIG_DIR=/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T//fm-lab.45N9WT/store-work
pid=27019 comm=claude
CLAUDE_CODE_USE_BEDROCK=1
HOME=/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T//fm-lab.45N9WT/userhome
CLAUDE_CONFIG_DIR=/var/folders/q4/nft8bs656qjb8k7bcjd_tt8r0000gn/T//fm-lab.45N9WT/store-personal
