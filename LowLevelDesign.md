1. Librabry delay
```
import java.util.concurrent.*;

class TaskHandler {
    private final ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
    private final ConcurrentLinkedQueue<Runnable> taskQueue = new ConcurrentLinkedQueue<>();

    public void schedule(Runnable task, long delayMillis) {
        long submissionTime = System.currentTimeMillis();
        long executionTime = submissionTime + delayMillis;

        taskQueue.add(task);
        scheduler.schedule(() -> {
            if (System.currentTimeMillis() >= executionTime) {
                task.run();
                taskQueue.remove(task);
            }
        }, delayMillis, TimeUnit.MILLISECONDS);
    }

    public int getPendingTaskCount() {
        return taskQueue.size();
    }

    public void shutdown() {
        scheduler.shutdown();
    }

    // Example Methods
    public void printMessage(String message) {
        System.out.println("Message: " + message);
    }

    public void addNumbers(int a, int b) {
        System.out.println("Sum: " + (a + b));
    }

    public void fetchData() {
        System.out.println("Fetching data...");
        try { Thread.sleep(500); } catch (InterruptedException ignored) {}
        System.out.println("Data fetched!");
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        TaskHandler handler = new TaskHandler();

        handler.schedule(() -> handler.printMessage("Hello, World!"), 2000);
        handler.schedule(() -> handler.addNumbers(5, 10), 1000);
        handler.schedule(handler::fetchData, 3000);

        Thread.sleep(1500);
        System.out.println("Pending tasks: " + handler.getPendingTaskCount());

        Thread.sleep(3000);
        System.out.println("Pending tasks: " + handler.getPendingTaskCount());

        handler.shutdown(); // Shutdown the scheduler gracefully
    }
}

```
2. Design Hash Map
     ```
            class MyHashMap {
    static class Entry {
        String key;
        int value;
        Entry next;

        Entry(String key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private static final int SIZE = 16;
    private Entry[] buckets = new Entry[SIZE];

    private int getIndex(String key) {
        return Math.abs(key.hashCode()) % SIZE;
    }

    public void put(String key, int value) {
        int idx = getIndex(key);
        Entry head = buckets[idx];

        for (Entry curr = head; curr != null; curr = curr.next) {
            if (curr.key.equals(key)) {
                curr.value = value; // update
                return;
            }
        }

        Entry newEntry = new Entry(key, value);
        newEntry.next = head;
        buckets[idx] = newEntry;
    }

    public Integer get(String key) {
        int idx = getIndex(key);
        for (Entry curr = buckets[idx]; curr != null; curr = curr.next) {
            if (curr.key.equals(key)) return curr.value;
        }
        return null;
    }

    public void remove(String key) {
        int idx = getIndex(key);
        Entry curr = buckets[idx], prev = null;

        while (curr != null) {
            if (curr.key.equals(key)) {
                if (prev == null) buckets[idx] = curr.next;
                else prev.next = curr.next;
                return;
            }
            prev = curr;
            curr = curr.next;
        }
    }
    }

     ```
