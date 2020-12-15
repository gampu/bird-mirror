[![Build Status](https://travis-ci.com/gampu/bird-mirror.svg?branch=master)](https://travis-ci.com/gampu/bird-mirror)
# Welcome to the bird-mirror wiki!

## What's this?
- The [Bird Internet Routing Daemon](https://en.wikipedia.org/wiki/Bird_Internet_routing_daemon) already has an official [mirror on github](https://github.com/BIRD/bird), but it seems to be no longer being maintained. This repository is a mirror of [bird on gitlab](https://gitlab.nic.cz/labs/bird) that updates itself every day using [travis cron job](https://docs.travis-ci.com/user/cron-jobs/).

## How does it work?
- Travis uses the following steps to update this repository by fetching updates from [bird on gitlab](https://gitlab.nic.cz/labs/bird):
```yaml
dist: xenial
script:
 - git reset --soft HEAD~1
 - git pull https://gitlab.nic.cz/labs/bird.git master
 - git add --all
 - git commit -m "Travis Commit. Build No.- $TRAVIS_BUILD_NUMBER [skip ci]"
 - pwd
 - git log -3
 - git push -f https://${GH_TOKEN}@github.com/gampu/bird-mirror.git HEAD:master
```
  - `git reset --soft HEAD~1` helps get rid of the commit that includes the above file contents temporarily.
  - `git pull https://gitlab.nic.cz/labs/bird.git master` pulls any changes from the official repository. Note that only the master branch is being mirrored here.
  - Having pulled changes(if available), `git add --all` adds the travis config file back to the git staging area.
  - `git commit -m "Travis Commit. Build No.- $TRAVIS_BUILD_NUMBER [skip ci]"` adds a commit on top of the new pulled changes and updates the commit message. `TRAVIS_BUILD_NUMBER` is a number that can be used to identify a travis build. `[skip ci]` is needed to prevent travis from building a recently built code.
  - `pwd` and `git log -3` have been added for debugging purposes.
  - Finally, `git push -f https://${GH_TOKEN}@github.com/gampu/bird-mirror.git HEAD:master` pushes the updates to this repository. Note that, `-f` is needed, for the case, when there were no new updates available. `GH_TOKEN` is a token that is required for travis to push changes to this repo. It has been generated using [github-tokens](https://github.com/settings/tokens) and added as an environment variable using travis web interface.

[Discussions are welcome](https://github.com/gampu/bird-mirror/discussions) \
P.S: This repo doesn't intend to compile/build bird. It only intends to mirror the code from gitlab to github, something which is extremely difficult if you are not a member/contributor on gitlab.
