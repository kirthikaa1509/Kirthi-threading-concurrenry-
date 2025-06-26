# Kirthi-threading-concurrenry-
import threading
import time
import asyncio

# Shared resource and lock for thread-safe access
counter = 0
lock = threading.Lock()

# 🔹 Task 1: Multithreaded counter increment with synchronization
def thread_task(name):
    global counter
    for _ in range(3):
        time.sleep(1)
        with lock:
            counter += 1
            print(f"[{name}] Incremented counter to {counter}")

# 🔹 Task 2: Async task using asyncio
async def async_task(name, delay):
    print(f"⏳ {name} started, will run for {delay} seconds...")
    await asyncio.sleep(delay)
    print(f"✅ {name} finished!")

# 🔹 Task 3: Run asyncio tasks
def run_asyncio_tasks():
    async def main():
        await asyncio.gather(
            async_task("Async-1", 2),
            async_task("Async-2", 3)
        )
    asyncio.run(main())

# 🔹 Driver code
if __name__ == "__main__":
    print("🔧 Starting multithreaded tasks...\n")

    t1 = threading.Thread(target=thread_task, args=("Thread-1",))
    t2 = threading.Thread(target=thread_task, args=("Thread-2",))

    t1.start()
    t2.start()

    t1.join()
    t2.join()

    print(f"\n🔁 Final counter value after threading: {counter}\n")

    print("⚙️ Running async tasks...\n")
    run_asyncio_tasks()

    print("\n🎉 All tasks completed.")
