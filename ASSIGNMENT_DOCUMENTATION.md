# Assignment 3 - Complete Documentation

**Student Name**: [ Hessa Abdulaziz Al-Huwailsh ]  
**Student ID**: [ 445052136 ]  
**Date Submitted**: [7-5-2026]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [https://drive.google.com/file/d/1uLMvhUE15p49XaSqYFaQa0iuN1eFAlJh/view?usp=sharing]

**Video filename**: `[445052136_Assignment3_Synchronization.mp4]`

**Verification**:
- [x] Link is accessible (tested in incognito mode)
- [x] Video is 3-5 minutes long
- [x] Video shows code walkthrough and commits
- [x] Video has clear audio
- [x] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 3, 2026 - 12:59 AM ]
**What I implemented**: 
I updated the student ID in the main method to `445052136`.

**Challenges encountered**: 
The main challenge was understanding the starter code structure and making sure I only changed the required student ID value.

**How I solved it**: 
I searched for the `studentID` variable inside the `main()` method and replaced the default value with my actual student ID.

**Testing approach**: 
I ran the program and checked the output header to confirm that my student ID appeared correctly.

**Time spent**: 
30 minutes

---

### Entry 2 - [May 5, 2026 - 10:35 PM ]
**What I implemented**: 
I added a `ReentrantLock` named `counterLock` to protect the shared counter variables: `contextSwitchCount`, `completedProcessCount`, and `totalWaitingTime`.

**Challenges encountered**: 
At first, I accidentally left some increment statements outside the critical section.

**How I solved it**: 
I moved all counter updates inside `try-finally` blocks protected by `counterLock`.

**Testing approach**: 
I ran the program and verified that the completed process count matched the total number of processes created.

**Time spent**: 
1.5 hours

---

### Entry 3 - [May 5, 2026 - 11:37 PM]
**What I implemented**: 
I added another `ReentrantLock` named `logLock` to protect the shared `executionLog`.

**Challenges encountered**: 
The execution log uses `ArrayList`, which is not thread-safe when accessed by multiple threads simultaneously.

**How I solved it**: 
I protected the `executionLog.add(message)` operation using `logLock.lock()` and `logLock.unlock()` inside a `finally` block.

**Testing approach**: 
I ran the program several times and checked that the execution log summary appeared correctly without exceptions.

**Time spent**: 
1 hour

---

### Entry 4 - [ May 6, 2026 - 12:46 AM]
**What I implemented**: 
I implemented a binary semaphore named `cpuSemaphore` with one permit to control access to the simulated CPU.

**Challenges encountered**: 
The `run()` method contained multiple nested `try` blocks, so identifying the correct location for the semaphore was slightly confusing.

**How I solved it**: 
I placed `acquireUninterruptibly()` inside the outer `try` block and released the semaphore inside the outer `finally` block.

**Testing approach**: 
I tested the program output to confirm that all processes completed correctly and no synchronization issues appeared.

**Time spent**: 
2 hours

---

### Entry 5 - [May 7, 2026 - 2:00 AM ]
**What I implemented**: 
I finalized the documentation, reviewed the technical questions, verified the output values, and prepared the video demonstration.

**Challenges encountered**: 
The main challenge was making sure the documentation accurately reflected the implemented synchronization mechanisms and matched the GitHub commit history.

**How I solved it**: 
I reviewed the code, commits, and final output carefully before completing the documentation.

**Testing approach**: 
I executed the program multiple times and verified the final statistics and execution logs.

**Time spent**: 
1.5 hours
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[One race condition in the original code was related to the shared counter variables, such as `contextSwitchCount`, `completedProcessCount`, and `totalWaitingTime`. These variables are shared between process threads, and operations like `contextSwitchCount++` are not atomic because they include reading, updating, and writing the value back. If two threads update the same counter at the same time, one update may be lost and the final result may become incorrect.

Another race condition was related to `executionLog`, which is a shared `ArrayList`. `ArrayList` is not thread-safe, so if multiple threads add messages at the same time, the log may become inconsistent or cause errors such as `ConcurrentModificationException`. This could make the final execution log summary unreliable.
]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[A `ReentrantLock` is used to protect a critical section so only one thread can access shared data at a time. In my code, I used `counterLock` to protect the shared counters and `logLock` to protect the shared execution log. This prevents multiple threads from modifying the same shared data at the same time.

A `Semaphore` is used to control how many threads can access a limited resource at the same time. I used a binary semaphore named `cpuSemaphore` with one permit to control access to the simulated CPU. This means only one process can execute on the CPU at a time. In simple terms, locks protect shared data, while semaphores control access to shared resources.
]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[Deadlock happens when two or more threads wait forever because each thread is holding a resource and waiting for another resource to be released. One prevention technique is to release locks inside a `finally` block so the lock is released even if an exception happens. Another prevention technique is to avoid holding multiple locks unnecessarily or using locks in a confusing order.

In my implementation, I used `try-finally` with `counterLock`, `logLock`, and the semaphore release. This makes sure that each lock or semaphore permit is released after the critical section finishes. I also kept the critical sections short, which reduces the chance of threads blocking each other for a long time.
]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[For Task 1, I used one lock called `counterLock` for the three shared counter variables. This is a coarse-grained locking approach because the same lock protects `contextSwitchCount`, `completedProcessCount`, and `totalWaitingTime`. I chose this approach because the counter updates are very short, and using one lock makes the code easier to read and less complex.

The trade-off is that coarse-grained locking may reduce concurrency because only one counter can be updated at a time. Fine-grained locking, where each counter has its own lock, can provide better concurrency because the counters are independent. However, fine-grained locking also makes the code more complex and harder to maintain. For this assignment, I chose the simpler and safer design because correctness was the main goal.
 - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
`contextSwitchCount`, `completedProcessCount`, and `totalWaitingTime`.

**Why they need protection**: 
These variables are shared by multiple threads. If more than one thread updates them at the same time, the final values may become incorrect because the update operations are not atomic.

**Synchronization mechanism used**: 
`ReentrantLock counterLock`.

**Code snippet**:

```java
// Lock used to protect shared counter variables
public static final ReentrantLock counterLock = new ReentrantLock();

public static void incrementContextSwitch() {
    // Enter critical section for context switch counter
    counterLock.lock();
    try {
        contextSwitchCount++;
    } finally {
        // Always release the lock to avoid deadlock
        counterLock.unlock();
    }
}
// Paste your implementation here
```


public static void incrementCompletedProcess() {
    // Enter critical section for completed process counter
    counterLock.lock();
    try {
        completedProcessCount++;
    } finally {
        // Always release the lock to avoid deadlock
        counterLock.unlock();
    }
}

public static void addWaitingTime(long time) {
    // Enter critical section for total waiting time
    counterLock.lock();
    try {
        totalWaitingTime += time;
    } finally {
        // Always release the lock to avoid deadlock
        counterLock.unlock();
    }
}
**Justification**: 

The lock ensures that only one thread can update the shared counters at a time. This prevents lost updates and keeps the final statistics correct.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog, which is a shared ArrayList.

**Why it needs protection**: 
ArrayList is not thread-safe. If multiple threads add messages at the same time, the log can become inconsistent or cause runtime errors.

**Synchronization mechanism used**: 
ReentrantLock logLock.

**Code snippet**:
```java
// Paste your implementation here
```
// Lock used to protect the shared execution log
public static final ReentrantLock logLock = new ReentrantLock();

public static void logExecution(String message) {
    // Enter critical section for execution log
    logLock.lock();
    try {
        executionLog.add(message);
    } finally {
        // Always release the lock to avoid deadlock
        logLock.unlock();
    }
}

**Justification**: 
The lock makes the log update safe by allowing only one thread to add a message at a time.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
The purpose of the semaphore is to control access to the simulated CPU.
**Number of permits and why**: 
I used one permit because the simulation represents one CPU, so only one process should execute at a time.
**Where implemented**: 
The semaphore is declared in SharedResources and used inside the run() method and runToCompletion() method in the Process class.
**Code snippet**:
```java
// Binary semaphore used to allow only one process to access the CPU at a time
public static final Semaphore cpuSemaphore = new Semaphore(1);
// Tracks whether this process acquired the CPU permit
boolean permitAcquired = false;

try {
    // Acquire CPU permit before process execution
    SharedResources.cpuSemaphore.acquireUninterruptibly();
    permitAcquired = true;

    // Process execution code runs here

} finally {
    // Release CPU permit after execution
    if (permitAcquired) {
        SharedResources.cpuSemaphore.release();
    }
}
// Paste your implementation here
```


**Effect on program behavior**: 
The semaphore ensures that only one process enters the CPU execution section at a time. This makes the CPU simulation safer and more controlled.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
```bash
# Commands used (run the program at least 5 times)
```

**Results**:
 ═══ Synchronization Statistics ═══
Total Context Switches: 24
Total Completed Processes: 12
Total Waiting Time: 565233ms
Average Waiting Time: 47102ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         5            5900         41501       
P2         1            2898         4103        
P3         4            8122         59928       
P4         5            8922         60058       
P5         5            4727         51519       
P6         3            9275         60992       
P7         4            2518         23206       
P8         2            9535         62283       
P9         5            5309         60337       
P10        2            6262         61657       
P11        4            3931         37855       
P12        5            3662         41794       

═══ Execution Log Summary ═══
Total log entries: 48
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)
Synchronization is necessary because shared counters and shared lists can be accessed by multiple threads at the same time. Without locks, the counter values may become incorrect because some updates may be lost. Without protecting the execution log, the shared ArrayList may be modified unsafely. The semaphore also helps control CPU access so the simulation behaves correctly.

**Conclusion**: 
The program completed successfully, and the synchronization mechanisms helped keep the final output consistent and correct.
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 
I ran the program several times and checked the console output until the final execution log summary appeared.
**Results**: 
No ConcurrentModificationException appeared. The program printed the execution log summary successfully.
**What this proves**: 
This proves that protecting executionLog.add(message) with logLock helps prevent unsafe concurrent access to the shared ArrayList.
---

### Test 3: Correctness Verification
**What I tested**:  Verifying correct final values (total burst time, context switches, completed processes, and execution log entries)

**Expected values**: 
- Total Completed Processes should equal 12 because the simulation creates 12 processes.
- Total Context Switches should remain consistent across executions.
- Total log entries should remain consistent because every process execution event is logged.

**Actual values**: 
Based on the program output:
- Total Context Switches: 24
- Total Completed Processes: 12
- Total Waiting Time: approximately 565000 ms
- Average Waiting Time: approximately 47100 ms
- Total log entries: 48

**Analysis**: 
The output values were correct and consistent across multiple executions. All 12 processes completed successfully, and the number of context switches remained stable. The waiting time values changed slightly between runs because thread scheduling depends on execution timing, which is normal in multithreaded programs. The execution log also remained consistent with 48 log entries in each execution.
---

### Test 4: Different Scenarios
**Scenario tested**: [Running the program multiple times using the same student ID]

**Purpose**: The purpose was to verify that the program remains stable and does not fail due to synchronization errors.

**Results**: The program completed successfully each time and printed the final statistics.

**What I learned**: 
I learned that synchronization is important even if the program appears to work normally. Race conditions may not happen every time, but they can still occur without proper protection.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[I learned that synchronization is very important in multithreaded programs because multiple threads can access the same shared resource. I learned that even a simple operation like count++ is not completely safe because it is not atomic. Using ReentrantLock helped me understand how to protect critical sections manually. I also learned that ArrayList is not thread-safe, so it should be protected when multiple threads use it. The semaphore helped me understand how limited resources, such as CPU access, can be controlled. The most important part was using try-finally so locks and semaphore permits are always released. This assignment helped me connect the theory of process synchronization with a practical Java program.]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
Online banking systems need synchronization when multiple transactions update the same account balance.

**Example 2**: 
Ticket booking systems need synchronization when many users try to reserve the same seat at the same time.
---

### How I would explain synchronization to others:

[Synchronization is like using a key for a shared room. If many people want to enter the room and change something important, only one person should enter at a time. In programming, the shared room is the critical section, and the key is the lock. Without synchronization, two threads may change the same data at the same time and produce wrong results. A semaphore is similar, but it can have a limited number of permits depending on how many threads are allowed to use the resource.]

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/hassahAziz/OS-Assignment3-hassah-aziz.git

**Number of commits**: 5 commits

**Commit messages**: 
1. The student ID number has been changed to 445052136
2. Add ReentrantLock for shared counters
3. Protect execution log with ReentrantLock
4. Add binary semaphore for CPU synchronization
5. Complete assignment documentation and video preparation
---

## Summary

**Total time spent on assignment**: 5 days

**Key takeaways**: 
1. Shared variables must be protected to prevent race conditions.
2. ReentrantLock is useful for protecting critical sections.
3. Semaphore is useful for controlling access to limited resources like CPU access.


**Most challenging aspect**: 
The most challenging aspect was placing the semaphore in the correct outer try-finally block because the code had multiple try blocks.

**What I'm most proud of**: 
I am most proud that I understood the race conditions and fixed them using synchronization mechanisms.
---

**End of Documentation**
