https://www.freertos.org/implementation/a00005.html

# Scheduling

<!--
The scheduler is the part of the kernel responsible for deciding which task should be executing at any particular time. The kernel can suspend and later resume a task many times during the task lifetime.
-->

スケジューラは、特定の時間にどのタスクを実行すべきかを決めるカーネルの一部です。カーネルは、タスクの存続期間中に何度もタスクを中断し、後で再開することができます。

<!--
The scheduling policy is the algorithm used by the scheduler to decide which task to execute at any point in time. The policy of a (non real time) multi user system will most likely allow each task a "fair" proportion of processor time. The policy used in real time / embedded systems is described later.
-->

スケジューリングポリシーは、スケジューラーが、どの時点でどのタスクを実行するかを決定するために使用するアルゴリズムです。 （非リアルタイム）マルチユーザシステムのポリシーは、各タスクをプロセッサ時間の「公平」な割合にする可能性が高いでしょう。リアルタイム / 組み込みシステムで使用されるポリシーについては、後で説明します。

<!--
In addition to being suspended involuntarily by the kernel a task can choose to suspend itself. It will do this if it either wants to delay (sleep) for a fixed period, or wait (block) for a resource to become available (eg a serial port) or an event to occur (eg a key press). A blocked or sleeping task is not able to execute, and will not be allocated any processing time.
-->

カーネルによって我知らず中断されることに加えて、タスクは自身を中断することもできます。これは、一定期間遅延（スリープ）するか、リソースが使用可能になる（例えばシリアルポート）か、イベントが発生するのを待つ（ブロック）場合（キー押下など）に行います。ブロックされたタスクまたはスリープしているタスクは実行できず、処理時間も割り当てられません。

![image](https://www.freertos.org/implementation/suspending.gif)

<!--
Referring to the numbers in the diagram above:

At (1) task 1 is executing.
At (2) the kernel suspends (swapps out) task 1 ...
... and at (3) resumes task 2.
While task 2 is executing (4), it locks a processor peripheral for its own exclusive access.
At (5) the kernel suspends task 2 ...
... and at (6) resumes task 3.
Task 3 tries to access the same processor peripheral, finding it locked task 3 cannot continue so suspends itself at (7).
At (8) the kernel resumes task 1.
Etc.
The next time task 2 is executing (9) it finishes with the processor peripheral and unlocks it.
The next time task 3 is executing (10) it finds it can now access the processor peripheral and this time executes until suspended by the kernel.
-->
