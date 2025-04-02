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
