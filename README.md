# processUtils

Lightweight utilities for launching and managing subprocesses in Python with basic support for asynchronous stdout/stderr consumption.

## Overview

`processUtils` provides a thin wrapper around Python’s `subprocess.Popen`, aiming to simplify process execution and enable non-blocking access to a process’s output streams. It uses background threads and queues to continuously read `stdout` and `stderr`, making it possible to inspect output while the process is still running.

## Features

- Start subprocesses with a simple interface
- Asynchronous reading of `stdout` and `stderr`
- Accumulated output accessible during execution
- Basic process lifecycle management (start, terminate, kill)
- Minimal abstraction over `subprocess.Popen`

## Limitations

This project was originally developed for Python 2 and has several constraints:

- **Python 2-oriented**: Requires adaptation for Python 3 (e.g., `Queue` → `queue`, print syntax)
- **Limited portability**: Uses `os.setsid`, making it Unix-specific
- **Memory usage**: Stores full stdout/stderr in memory, which may not scale for verbose processes
- **Latency**: Output updates rely on polling with `sleep`, introducing delays
- **Partial interactivity**: Input handling is not designed for continuous interaction
- **Dependencies**: Relies on a non-standard logging module (`logengine`)
- **Maintenance status**: Not actively maintained or production-hardened

## When to Use

This library may be useful for:

- Simple automation scripts
- Internal tooling where lightweight process control is sufficient
- Situations where real-time access to subprocess output is needed without complex setup

## When Not to Use

Consider modern alternatives if you need:

- Full Python 3 compatibility
- High-performance or large-scale process handling
- Robust cross-platform behavior
- True asynchronous I/O

Recommended alternatives:

- `subprocess.run` (simple use cases)
- `subprocess.Popen` (manual control)
- `asyncio.create_subprocess_exec` (async workflows)

## Example

```python
from processUtils import Process

p = Process("ls -la")
p.start()

while p.running:
    print(p.stdout)

print("Process finished with code:", p.returncode)

## Notes
This project reflects a pragmatic approach to subprocess handling at the time it was written. While still functional for certain use cases, it is not intended to compete with modern, actively maintained solutions.

## License
This project is licensed under the MIT License.
