https://www.freertos.org/implementation/a00007.html

# Real Time Applications

<!--
Real time operating systems (RTOSes) achieve multitasking using these same principals - but their objectives are very different to those of non real time systems. The different objective is reflected in the scheduling policy. Real time / embedded systems are designed to provide a timely response to real world events. Events occurring in the real world can have deadlines before which the real time / embedded system must respond and the RTOS scheduling policy must ensure these deadlines are met.
-->

リアルタイムオペレーティングシステム（RTOSes）は、これらの同じプリンシパルを使用してマルチタスキングを実現しますが、その目的は非リアルタイムシステムのものと大きく異なります。異なる目的がスケジューリング方針に反映されます。リアルタイム/組込みシステムは、実世界のイベントにタイムリーに対応するように設計されています。現実世界で発生するイベントは、リアルタイム/組み込みシステムが応答しなければならない期限を有することができ、RTOSスケジューリングポリシーはこれらの期限を確実に満たされなければならない。

<!--
To achieve this objective the software engineer must first assign a priority to each task. The scheduling policy of the RTOS is then to simply ensure that the highest priority task that is able to execute is the task given processing time. This may require sharing processing time "fairly" between tasks of equal priority if they are ready to run simultaneously.
-->

この目的を達成するために、ソフトウェアエンジニアはまず各タスクに優先順位を割り当てる必要があります。 RTOSのスケジューリングポリシーとは、実行可能な最優先タスクが、処理時間を与えられたタスクであることを単に保証することです。同時に実行する準備ができている場合、等しい優先度のタスク間で処理時間を「公平に」共有する必要があるかもしれません。

## Example:

<!--
The most basic example of this is a real time system that incorporates a keypad and LCD. A user must get visual feedback of each key press within a reasonable period - if the user cannot see that the key press has been accepted within this period the software product will at best be awkward to use. If the longest acceptable period was 100ms - any response between 0 and 100ms would be acceptable. This functionality could be implemented as an autonomous task with the following structure:
-->

最も基本的な例は、キーパッドとLCDを組み込んだリアルタイムシステムです。合理的な期間内に各キープレスの視覚的フィードバックを得る必要があります。ユーザーがキープレスがこの期間内に受け入れられたことが分からない場合、ソフトウェア製品は使用するのが厄介です。許容可能な最長期間が100msの場合、0〜100msの間の応答は許容されます。この機能は、以下の構造を持つ自律型タスクとして実装できます。

```c
void vKeyHandlerTask( void *pvParameters )
{
    // Key handling is a continuous process and as such the task
    // is implemented using an infinite loop (as most real time
    // tasks are).
    for( ;; )
    {
        [Suspend waiting for a key press]

        [Process the key press]
    }
}
```

<!--
Now assume the real time system is also performing a control function that relies on a digitally filtered input. The input must be sampled, filtered and the control cycle executed every 2ms. For correct operation of the filter the temporal regularity of the sample must be accurate to 0.5ms. This functionality could be implemented as an autonomous task with the following structure:
-->

今、リアルタイムシステムがデジタルフィルタリングされた入力に依存する制御機能を実行していると仮定します。入力はサンプリングされ、フィルタリングされ、制御サイクルは2msごとに実行されなければなりません。フィルタの正確な動作のために、サンプルの時間的規則性は0.5msまで正確でなければならない。この機能は、以下の構造を持つ自律型タスクとして実装できます。

```c
void vControlTask( void *pvParameters )
{
    for( ;; )
    {
        [Suspend waiting for 2ms since the start of the previous
        cycle]

        [Sample the input]
        [Filter the sampled input]
        [Perform control algorithm]
        [Output result]
    }
}
```

<!--
The software engineer must assign the control task the highest priority as:
The deadline for the control task is stricter than that of the key handling task.
The consequence of a missed deadline is greater for the control task than for the key handler task.
The next page demonstrates how these tasks would be scheduled by a real time operating system.
-->

ソフトウェアエンジニアは、制御タスクに次のような最高の優先順位を割り当てる必要があります。

1. 制御タスクのデッドラインは、キー操作タスクのデッドラインよりも厳しくなっています。
2. デッドラインの欠落の結果は、制御タスクにとってキーハンドラタスクよりも大きい。

次のページは、これらのタスクがリアルタイムオペレーティングシステムによってスケジュールされる方法を示しています。
