# 6. Loops

Repeat a block of code.

### `while` Loop

Executes as long as a condition is **true**.

```bash
while [[ condition ]]; do
  # commands
done
```
**Example:** An infinite loop that sleeps for 2 seconds.
```bash
while true; do
  echo "Looping..."
  sleep 2
done
```

**Counter-based `while` loop:**

```bash
#!/bin/bash

counter=1
while [[ "$counter" -le 5 ]]; do
    echo "Iteration $counter"
    ((counter++))
done
```

### `until` Loop

Executes as long as a a condition is **false**.

```bash
until [[ condition ]]; do
  # commands
done
```
**Example:** Wait until a file has content.
```bash
until [[ -s "file.txt" ]]; do
  echo "File is empty..."
  sleep 2
done
```

**Wait for a service to be up:**

```bash
#!/bin/bash

until nc -z localhost 8080; do
    echo "Waiting for service on port 8080..."
    sleep 1
done

echo "Service is up!"
```

### `for` Loop

Iterates over a sequence of items.

```bash
for var_name in ITEM1 ITEM2 ITEM3; do
  # commands using $var_name
done
```
**Example:** Iterate through command-line arguments.
```bash
for ip in "$@"; do
  echo "Processing ${ip}"
done
```

**Iterate over a range of numbers:**

```bash
#!/bin/bash

for i in {1..5}; do
    echo "Number: $i"
done
```

### Loop Controls

-   `break`: Exits the current loop immediately.
-   `continue`: Skips the rest of the current iteration and proceeds to the next one.

**Example of `break` and `continue`:**

```bash
#!/bin/bash

for i in {1..10}; do
    if [[ "$i" -eq 3 ]]; then
        continue  # Skip this iteration
    fi
    echo "Number: $i"
    if [[ "$i" -eq 8 ]]; then
        break  # Exit the loop
    fi
done
```
