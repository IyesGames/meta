# Loopless

**This is already implemented! See: [`iyes_loopless`](https://github.com/IyesGames/iyes_loopless).**

Implementation of States/RunCriteria/FixedTimestep, inspired by the Stageless RFC.

Stageless Schedules / Exclusive Systems, can be mapped to Stages.

"Atomic run criteria" can be implemented as a special custom `System` impl,
similar to Bevy's "system chaining".

## Further Developments

Probably no point in spending any time on this, given that Stageless is
ComingSoon™.

But we *could* do some things to bring us even more benefits:

### LooplessStage

Stage type to replace Bevy `SystemStage`.

Rip out all the complexity related to run criteria and looping.

Implement executing run-conditions in-line, to avoid spawning
tasks for systems that should not run.

This will be the behavior in stageless, but we could implement
it in loopless as a custom stage type.

### StagelessStage ;)

One step further: allow exclusive systems and Commands application
in the middle of a stage, between parallel systems.

At this point we will have basically implemented Stageless. *hehe*

(but contained inside of a stage! *grin* )
