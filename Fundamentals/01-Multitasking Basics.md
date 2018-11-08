https://www.freertos.org/implementation/a00004.html

# Multitasking

<!--
The kernel is the core component within an operating system. Operating systems such as Linux employ kernels that allow users access to the computer seemingly simultaneously. Multiple users can execute multiple programs apparently concurrently.
Each executing program is a task (or thread) under control of the operating system. If an operating system can execute multiple tasks in this manner it is said to be multitasking.
-->

カーネルは、オペレーティングシステム内のコアコンポーネントです。 Linuxなどのオペレーティングシステムでは、カーネルを採用しているため、複数のユーザがコンピュータに一見同時にアクセスできます。複数のユーザーは複数のプログラムを明らかに並行して実行できます。 (*※seemingly simultaneously と apparently concurrently いい感じの韻かつ正確な意味で訳したいが難しい*) 各実行プログラムは、オペレーティングシステムの制御下にあるタスク（またはスレッド）です。オペレーティングシステムがこのように複数のタスクを実行できる場合は、マルチタスクと言います。

<!--
The use of a multitasking operating system can simplify the design of what would otherwise be a complex software application:

-->

マルチタスキング・オペレーティング・システムを使用すると、複雑なソフトウェア・アプリケーションの設計を単純化できます。

<!--
The multitasking and inter-task communications features of the operating system allow the complex application to be partitioned into a set of smaller and more manageable tasks.

The partitioning can result in easier software testing, work breakdown within teams, and code reuse.

Complex timing and sequencing details can be removed from the application code and become the responsibility of the operating system.
-->

* オペレーティングシステムのマルチタスクおよびタスク間通信機能により、複雑なアプリケーションをより小さく管理しやすいタスクに分割することができます。
* パーティショニングによって、ソフトウェアのテスト、チーム内の作業の中断、コードの再利用が容易になります。
* 複雑なタイミングとシーケンシングの詳細は、アプリケーションコードから削除し、オペレーティングシステムの責任となります。

<!--See also the FAQ "Why use an RTOS?".-->

FAQ「RTOSを使う理由」も参照してください。

## Multitasking Vs Concurrency

<!--
A conventional processor can only execute a single task at a time - but by rapidly switching between tasks a multitasking operating system can make it appear as if each task is executing concurrently. This is depicted by the diagram below which shows the execution pattern of three tasks with respect to time. The task names are color coded and written down the left hand. Time moves from left to right, with the colored lines showing which task is executing at any particular time. The upper diagram demonstrates the perceived concurrent execution pattern, and the lower the actual multitasking execution pattern.
-->

従来のプロセッサは一度に1つのタスクしか実行できませんが、タスク間を迅速に切り替えることで、マルチタスク・オペレーティング・システムでは各タスクが同時に実行されているかのように見えるようになります。これは、時間に関する3つのタスクの実行パターンを示す下の図で表されます。タスク名は色分けされ、左手に書き込まれます。時間は左から右に移動し、色分けされた線は特定の時間に実行中のタスクを示します。

![image](https://www.freertos.org/implementation/TaskExecution.gif)
