# Executors Introduction

## Learning Goals

- Explain why we need abstractions over Threads.
- Explain tasks and executors.

## Why Do We Need Thread Abstractions?

We have been creating and running thread objects manually either by extending
the `Thread` class or by implementing the `Runnable` interface. It’s fairly
simple to create and run multiple threads when we’re dealing with small
programs. However, if you were to write an enterprise-grade application with
thousands of threads, it would quickly get overwhelming.

Java provides the Executor framework as the primary abstraction for executing
logical units of tasks. This allows us to define tasks and let the computer
handle thread management.

## What is a Task?

A task is a logical unit of work which can be represented as an object of a
class that implements the `Ruannable` interface. Each task is usually
independent of other tasks so that it can be completed by a single thread.

The classes in the Executor framework split task submission and task execution
into separate operations.

## Executors

The Executors in Java defines one or more threads into a single pool and puts
submitted tasks in a queue which are then executed by threads in the thread
pool.

![ExecutorService Diagram](https://curriculum-content.s3.amazonaws.com/java-mod-5/executor-service-diagram.png)

We can specify different policies for task execution when using the executors.
There are three executor interfaces that classes can implement for thread
lifecycle management:

- `Executor`: this is a simple interface for running new tasks.
- `ExecutorService`: this extends the `Executor` interface and adds features for
  managing task and executor lifecycle management. We’ll primarily be using this
  interface.
- `ScheduledExecutorService`: this is used for scheduling tasks in the future or
  tasks that need to execute on a fixed interval.

## Thread Pools

A thread pool in Java is an implementation of any of the executor interfaces.
These allow us to separate task submission and execution.

Thread pools contain worker threads that are assigned to a task. Once a worker
thread completes a task, it returns to the thread pool. A queue is used to track
tasks that need to be executed. We can specify the number of threads in the pool
to allow the executor to automatically scale based on the number of tasks.

## Running Your First Executor

Let’s run a simple example using the `Executor` interface.

```java
import java.util.concurrent.Executor;

class Example {

    public static void main(String[] args) {
        SimpleExecutor executor = new SimpleExecutor();
        MyTask myTask = new MyTask();
        executor.execute(myTask);
    }

    static class SimpleExecutor implements Executor {
        public void execute(Runnable runnable) {
            Thread newThread = new Thread(runnable);
            newThread.start();
        }
    }

    static class MyTask implements Runnable {
        public void run() {
            System.out.println("MyTask is running");
        }
    }

}
```

Notice that we simply pass the `myTask` to the executor and ask it to execute
the task for us. At no point do we have to create or start new threads
ourselves. This can make it significantly easier to write concurrent code while
also making our code more maintainable.

## Conclusion

We have learned about the Executor framework in Java that allows us to create
multi-threaded programs more easily. Next, we will learn more about executors
and how to use them.
