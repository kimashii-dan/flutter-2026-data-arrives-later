# NOTES

## Level 1 — order of events

Predicted:

```
A
C
(first frame)
B
6 students
```

Actual:

```
A
C
(first frame)
fetchStudents #1
B
6 students
```

`main()` ran to the end without waiting, the first frame was drawn, and two seconds later the timer callbacks came as events.

Calling `fetchStudents()` three times in a row, the third one printed:

```
Unhandled Exception: Instance of 'NoConnectionException'
```

and the app kept running.

With `sleep(const Duration(seconds: 2))` instead of `await Future.delayed(...)` the first frame was not drawn for two seconds: the loop was blocked, not waiting.

## Level 2 — without `if (!mounted) return;`

```
setState() called after dispose(): _StudentsScreenState#a1b2c(lifecycle state: defunct, not mounted)
```

## Level 4 — countdown

- Version B without `_sub.cancel()`, Back at 7: one second later `setState() called after dispose(): _VersionBState#d3e4f(lifecycle state: defunct, not mounted)`, and the console kept printing tick 6, tick 5 … tick 0 with nobody on screen. With `cancel()` the console stops at tick 7.
- Version A (StreamBuilder, no cancel in my code), Back at 7: the console stops at tick 7. StreamBuilder cancelled the subscription for me.

## Level 5 — un-awaited save

```
Unhandled Exception: Bad state: server rejected 6
```

The user saw a success.
