# 3.7. Thread Dump

The Thread Dump screen lets you inspect the currently ctive threads on your server.

Each thread is listed and access to the stacktraces is available where applicable. Icons to the left indicate the state of the thread: for example, threads with a green check-mark in a green cicle are in "RUNNABLE" state. On th right of the thread name, a down-arrow means you can expan to see the stacktrace for that thread.

When you move your cursor over a thread name, a box floats over the name with the state for that thread. Thread state can be:

|State|Meaning|
|---|---|
|NEW|A thread that has not yet started|
|RUNNABLE|A thread executing in the Java vitual machine.|
|BLOCKED|A thread that is blocked waiting for a monitor lock|
|WAITING|A thread that is waiting indefinitely for another thread to perform a particular action|
|TIMED_WAITING|A thread thatis waiting for another thread to perform an action for up to a specified waiting time|
|TERMINATED|A thread that has exited|

When you click on one of the threads that can be expanded, you'll see the stacktrace, as in the example below:

You can slso check the Show all stacktraces button to automatically enable expansion for all threads.