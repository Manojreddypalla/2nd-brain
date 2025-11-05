# 7. `case` Statements

A cleaner way to test multiple conditions against a single value, similar to `switch` in other languages.

**Syntax:**
```bash
case EXPRESSION in
  PATTERN1)
    # commands
    ;;
  PATTERN2)
    # commands
    ;;
  *)
    # default commands
    ;;
esac
```
-   `;;`: Terminates each case block.
-   `*`: A wildcard pattern for a default case.

**Example:**

```bash
#!/bin/bash

case "$1" in
    start)
        echo "Starting the service..."
        ;;
    stop)
        echo "Stopping the service..."
        ;;
    restart)
        echo "Restarting the service..."
        ;;
    *)
        echo "Usage: $0 {start|stop|restart}"
        ;;
esac
```
