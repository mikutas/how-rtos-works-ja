# Real Time Scheduling

<!--
The diagram below demonstrates how the tasks defined on the previous page would be scheduled by a real time operating system. The RTOS has itself created a task - the idle task - which will execute only when there are no other tasks able to do so. The RTOS idle task is always in a state where it is able to execute.
-->

以下の図は、前のページで定義されたタスクがリアルタイムオペレーティングシステムによってどのようにスケジューリングされるかを示しています。 RTOS自体はタスクを作成しています。アイドルタスクは他のタスクがない場合にのみ実行されます。 RTOSアイドルタスクは、常に実行可能な状態にあります。

![image](https://www.freertos.org/implementation/RTExample.gif)

上記の図を参照してください：

* 開始時には、2つのタスクのどちらも実行できません。vControlTask​​は、新しい制御サイクルを開始する正しい時刻を待っており、vKeyHandlerTaskはキーが押されるのを待っています。プロセッサ時間は、RTOSアイドルタスクに与えられます。
* 時間t1において、キーが押される。 vKeyHandlerTaskを実行できるようになりました。これはRTOSアイドルタスクより高い優先度を持ち、プロセッサ時間が与えられています。
* 時刻t2で、vKeyHandlerTaskがキーの処理とLCDの更新を完了しました。別のキーが押されて自身が中断し、RTOSアイドルタスクが再び再開されるまで続行できません。
* 時間t3において、タイマイベントは、次の制御サイクルを実行する時間であることを示す。 vControlTask​​を実行できるようになりました。また、最も優先度の高いタスクはプロセッサ時間のスケジューリングが即時に行われるようになりました。
* 時間t3とt4の間、vControlTask​​が実行中の間、キーを押す。 vKeyHandlerTaskは現在実行可能ですが、vControlTask​​より優先度が低いため、プロセッサ時間のスケジューリングは行われません。
* t4でvControlTask​​は制御サイクルの処理を完了し、次のタイマーイベントまで再起動できません。 vKeyHandlerTaskは、実行可能な最高の優先度を持つタスクになりました。前回のキーを処理するためにプロセッサ時間がスケジュールされています。
* t5でキー押下が処理され、vKeyHandlerTaskは次のキーイベントを待つために自身を中断する。再び私たちのタスクはどちらも実行可能でなくなり、RTOSアイドルタスクはプロセッサ時間を予定しています。
* t5とt6の間で、タイマーイベントは処理されますが、キー押下は発生しません。
* 次のキーを押すと時刻t6に発生しますが、vKeyHandlerTaskがキーの処理を完了する前にタイマーイベントが発生します。これで両方のタスクが実行可能状態になりました。 vControlTask​​は高い優先度を持つため、vKeyHandlerTaskはキーの処理を完了する前に中断され、vControlTask​​がプロセッサ時間のスケジューリングされます。
* t8で、vControlTask​​は制御サイクルの処理を完了し、次を待つために自身を中断する。 vKeyHandlerTaskは再び実行可能な最優先のタスクであり、キーの押下処理が完了できるようにプロセッサ時間がスケジュールされています。
