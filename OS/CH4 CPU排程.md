### CPU排程簡介
* CPU利用multiprogramming方法得到CPU最大使用率
* 一般process一開始都是CPU Burst後I/O Burst，先交由CPU處理該process，接著做I/O資料的傳送
* process會在此兩個狀態循環，最後呼叫system call中止process作為結束

CPU Sheduling從ready queue挑選process進入CPU執行，CPU sheduling選擇的process會在以下情況改變：
* running -> waiting (i/o or event wait)
* running -> ready (interrupt) e.g.time Sharing因為time-out回到ready
* waiting -> ready (i/o complete)
* terminated

1.preemptive可搶先核心
