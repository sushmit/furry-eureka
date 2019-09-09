# scheduleAtFixedRate vs scheduleWithFixedDelay

`ScheduledExecutorService` ensures that tasks to do **not** overlap.

If the scheduled task takes more time than input rate, then `scheduleAtFixedRate` keeps building queue.

```java

    @Test
    public void testRateAndDelay() {
        RunTask rate = new RunTask("=======================================FixedRate");
        RunTask delay = new RunTask("FixedDelay");

        Executors.newSingleThreadScheduledExecutor().scheduleAtFixedRate(rate, 0, 1, TimeUnit.SECONDS);
        Executors.newSingleThreadScheduledExecutor().scheduleWithFixedDelay(delay, 0, 1, TimeUnit.SECONDS);

        while (rate.getTaskCount() < 10 || delay.taskCount < 10);

    }

    private static class RunTask implements Runnable {
        private final long startTime = getSystemTimeInSeconds();

        private final long sleepTimeInMillis = 15000;
        private boolean flip = true;
        private volatile int taskCount = 0;
        private final String taskName;

        public RunTask(String taskName) {
            this.taskName = taskName;
        }

        public int getTaskCount() {
            return taskCount;
        }

        @Override
        public void run() {
            try {
                System.out.println(String.format("%s TaskStarting taskCount=%s timeSinceStart=%s", taskName, ++taskCount, getTimeSinceStart()));
                if (flip) {
                    Thread.sleep(sleepTimeInMillis);
                    flip = false;
                }
                System.out.println(String.format("%s TaskDone taskCount=%s", taskName, taskCount));
            } catch (InterruptedException e) {
                
            }
        }

        private long getTimeSinceStart() {
            return getSystemTimeInSeconds() - startTime;
        }

        private static long getSystemTimeInSeconds() {
            return TimeUnit.MILLISECONDS.toSeconds(System.currentTimeMillis());
        }
    }

```
