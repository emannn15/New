# Distributed Processing Deadlock Simulation

This project demonstrates a distributed processing deadlock scenario using Python's `multiprocessing` and `socket` modules. Two processes are simulated to compete for two locks (resources), which leads to a deadlock when each process holds one lock and waits for the other to be released.

## Requirements

- Python 3.x

## How It Works

1. **Lock Servers**:
   - Two lock servers are created, each representing a resource that can be "locked" by a process.
   - Each server listens on a different port (`6000` and `6001`).

2. **Processes**:
   - Two processes (`p1` and `p2`) are started.
   - Each process tries to acquire two locks but in reverse order, which causes a deadlock.
   - `p1` acquires `lock1` first and then tries to acquire `lock2`.
   - `p2` acquires `lock2` first and then tries to acquire `lock1`.

3. **Deadlock**:
   - Since each process holds one lock and waits for the other, neither can proceed, resulting in a deadlock.

## Code Explanation

### `process(lock1, lock2)`
This function simulates the behavior of a process in a distributed system. It:
- Connects to `lock1`, simulating the process of acquiring a lock.
- After holding `lock1` for a short time (`1 second`), it tries to connect to `lock2`.
- The deadlock occurs if another process is already holding `lock2`.

### `lock_server(addr)`
This function simulates a lock resource that processes can connect to. It:
- Listens on a given address (`localhost` and a specific port) for incoming connections.
- Simulates holding the resource for a set time (`5 seconds`) before releasing the lock.

## Running the Program

1. Make sure Python 3.x is installed on your system.
2. Copy the code from `deadlock.py` to your project folder.
3. Run the script:

```bash
python deadlock.py
