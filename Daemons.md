A *background process* that answers requests for services.

> In Windows, they are instead called *Services*.

Instead of running in the foreground like a standard program (where you can interact with it and close it), daemons *always runs in the background*. It waits silently for a specific event or request, then it wakes up to handle it.