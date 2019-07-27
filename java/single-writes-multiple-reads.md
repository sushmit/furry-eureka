# Memory Visibility: Single thread writes, multiple threads read

Changes made by one thread may never be visibile to another thread in the absence of all constructs which provide consitent view guarantees provided by the **Java memory model**.

The following program does not end. Ever. One way to fix the issue is to declare `subject` as `volatile`.

```java
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.ScheduledFuture;
import java.util.concurrent.TimeUnit;

public class OneThreadWritesMultipleReadAndMemoryErrors {

    private static final Object A = new Object();
    private static final Object B = new Object();
    private static Object subject = null;

    private static class Flipper {
        private boolean flag = true;
        public void flip() {
            if (flag) {
                flag = false;
                subject = B;
            } else {
                flag = true;
                subject = A;
            }
        }
    }

    private static class StopAndGo implements Runnable {
        @Override
        public void run() {
            while (subject == A) {
            }
            System.out.println("value");

            while (subject == B) {
            }
            System.out.println("value 2");

            while (subject == A) {
            }
            System.out.println("value");

            while (subject == B) {
            }
            System.out.println("value 2");
        }
    }

    public static void main(String[] args) throws InterruptedException {
        subject = A;
        Flipper flipper = new Flipper();

        Thread writer = new Thread(new StopAndGo());
        writer.start();

        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(1);
        ScheduledFuture scheduledFuture = scheduledExecutorService.scheduleWithFixedDelay(flipper::flip, 100, 100, TimeUnit.MILLISECONDS);

        writer.join();
        scheduledExecutorService.shutdownNow();
        scheduledFuture.cancel(true);
    }
}

```

### References
* [Memory Visibility explained under java.util.concurrent pacakge](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/package-summary.html#MemoryVisibility)
* [_happens-before_ relation order](https://docs.oracle.com/javase/specs/jls/se8/html/jls-17.html#jls-17.4.5)
* https://www.callicoder.com/java-concurrency-issues-and-thread-synchronization/
* https://en.wikipedia.org/wiki/Java_memory_model
