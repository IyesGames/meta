# Bevy Fork Management Script

Make a script to automatically make a bevy branch that is cool af :) and
compatible with all the plugins I want.

The idea is to have a version of Bevy that is as close to `main` as possible
without breaking any of the plugins I care about, and then optionally also
including a selection of unmerged PRs I like.

And to do this all automatically with minimal manual effort from me.

## Checking the Build

Clone each plugin I want to use and add a cargo patch to point it at the
WIP Bevy for testing.

## Make a Bevy that is close to main without breaking plugins

Take the latest bevy release.

Walk through the commit log between the release and main.

For each commit, try cherry-picking it. If it cannot be applied:
 - open a shell to allow for manual intervention (can just exit if i dont feel like fixing things)
 - when the shell is exited:
   - if the conflicts are resolved, apply the commit
   - if not, abort the cherry pick and move on

If the cherry pick was successful:
 - Try building bevy and running the tests
 - Try building each plugin, run its tests if it has any.

If any of this fails, undo the cherry-pick, drop that commit, and move on.

At the end of this all, we should have a Bevy branch with as many of the
commits as could be applied (some may have to be dropped because of previous
commits being dropped), without breaking the plugins or bevy.

## Add extra PRs on top

Walk through a list of PRs i'd like to have.

For each one:

 - Fetch it
 - Rebase it onto our new branch from before
   - if failed, give shell (as described before)
 - Build bevy and plugins and run tests, same as before

At the end of this all, we should hopefully have our desired PRs on top of
our custom branch.

## Profit

Behold! The ultimate Bevy! :)

I can just run this every once in a while when i feel like getting the newest
stuff, and force-push.
