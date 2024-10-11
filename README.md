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

The code consists of two main functions: `process()` and `lock_server()`.

### Code

```python
import multiprocessing
import socket
import time

def process(lock1, lock2):
    with socket.socket() as s1, socket.socket() as s2:
        s1.connect(lock1)
        print(f"{multiprocessing.current_process().name} got lock1")
        time.sleep(1)
        s2.connect(lock2)
        print(f"{multiprocessing.current_process().name} got lock2")

def lock_server(addr):
    with socket.socket() as server:
        server.bind(addr)
        server.listen(1)
        conn, _ = server.accept()
        time.sleep(5)
        conn.close()

if __name__ == "__main__":
    lock1 = ("localhost", 6000)
    lock2 = ("localhost", 6001)

    l1 = multiprocessing.Process(target=lock_server, args=(lock1,))
    l2 = multiprocessing.Process(target=lock_server, args=(lock2,))
    
    p1 = multiprocessing.Process(target=process, args=(lock1, lock2))
    p2 = multiprocessing.Process(target=process, args=(lock2, lock1))

    l1.start()
    l2.start()
    p1.start()
    p2.start()

    p1.join()
    p2.join()
    l1.join()
    l2.join()
