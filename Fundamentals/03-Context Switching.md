https://www.freertos.org/implementation/a00006.html

# Context Switching

<!--
As a task executes it utilizes the processor / microcontroller registers and accesses RAM and ROM just as any other program. These resources together (the processor registers, stack, etc.) comprise the task execution context.
-->

タスクが実行されると、それはプロセッサ/マイクロコントローラレジスタを利用し、他のプログラムと同様にRAMおよびROMにアクセスする。これらのリソース（プロセッサレジスタ、スタックなど）は、タスク実行**コンテキスト**を構成します。

![image](https://www.freertos.org/implementation/ExeContext.gif)

<!--
A task is a sequential piece of code - it does not know when it is going to get suspended (swapped out or switched out) or resumed (swapped in or switched in) by the kernel and does not even know when this has happened. Consider the example of a task being suspended immediately before executing an instruction that sums the values contained within two processor registers. While the task is suspended other tasks will execute and may modify the processor register values. Upon resumption the task will not know that the processor registers have been altered - if it used the modified values the summation would result in an incorrect value.
-->

タスクとは、カーネルによって中断（スワップアウトまたはスイッチアウト）または再開（スワップインまたはスイッチイン）されるタイミングを知らず、いつ起きたのかも知らない連続したコードです。 2つのプロセッサ・レジスタ内に含まれる値を合計する命令を実行する直前に、タスクが中断される例を考えてみる。タスクが中断されている間、他のタスクが実行され、プロセッサレジスタ値を変更することができる。再開時に、タスクはプロセッサレジスタが変更されたことを認識しません。変更された値を使用した場合、合計が不正確な値になります。

<!--
To prevent this type of error it is essential that upon resumption a task has a context identical to that immediately prior to its suspension. The operating system kernel is responsible for ensuring this is the case - and does so by saving the context of a task as it is suspended. When the task is resumed its saved context is restored by the operating system kernel prior to its execution. The process of saving the context of a task being suspended and restoring the context of a task being resumed is called context switching.
-->

このタイプのエラーを防ぐには、再開時にタスクの中断が直前のものと同じコンテキストを持つことが不可欠です。オペレーティングシステムのカーネルは、このような場合を確実にする責任があります。また、中断されているタスクのコンテキストを保存することもできます。タスクが再開されると、その保存されたコンテキストは、実行前にオペレーティングシステムカーネルによって復元されます。中断されているタスクのコンテキストを保存し、再開されているタスクのコンテキストを復元するプロセスは、コンテキスト切り替え（コンテキストスイッチング）と呼ばれます。
