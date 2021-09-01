### CPU排程簡介
* CPU利用multiprogramming方法得到CPU最大使用率
* 一般process一開始都是CPU Burst後I/O Burst，先交由CPU處理該process，接著做I/O資料的傳送
* process會在此兩個狀態循環，最後呼叫system call中止process作為結束

CPU Sheduling從ready queue挑選process進入CPU執行，CPU sheduling選擇的process會在以下情況改變：
* running -> waiting (i/o or event wait)
* running -> ready (interrupt) e.g.time Sharing因為time-out回到ready
* waiting -> ready (i/o complete)
* terminated

### Starvation
Def：process因為長期無法取得完工所需資源，導致遲遲無法完工，形成indefinite blocking現象(不確定何時結束)

### non-preemptive：
Def：除非執行中的process自願放棄CPU，其他process才有機會取得，否則只能等待，不可逕自搶奪CPU
優點：
1.content switching次數較少
2.Process完成時間可預期
3.比較不會發生race condition
缺點：
1.排班效能差(可能有護衛效應)
2.不適合用在time-sharing system即real-time system

### preemptive：
Def：執行中的process可能被迫放棄CPU回到ready queue等待，將CPU提供給別人使用
優點：
1.排班效能佳(waiting time、Turnaround time較少)
2.適合time-sharing system及real-time system
缺點：
1.content switch 次數較多
2.process完工時間不可預期
3.可能會發生race condition

CPU ultiliztion = cpu花在process time / cpu total time
Throughput：單位時間內完成的jobs數目
Waiting time：process在ready queue等待時間加總，直到獲得CPU控制
Turnaround time：process進入(到達)ready queue到他工作完成的這段時間差值
Response time：user 輸入命令/Data給系統~系統產生第一個回應之時間差

##CPU 排班法
### FCFS
Def：到達時間最小的process優先取得CPU
* 排班效能最差，avg waiting time與avg Turnaround time最常，結構似FIFO
* 可能有護衛效應：很多process均在等待一個很長的CPU time完成工作，造成平均等待時間大幅增加，對I/O bound process有很糟糕的影響，低I/O利用度
* 公平、簡單
* 沒有Starvation
* non-preemptive：不可插隊、不可搶先


### SJF
Def：具有最小的CPU time之process優先取得CPU
* non-preemptive：SJF
* preemptive：SRTF

優點：
1.較少waiting time(排班效益佳)
缺點：
1.不公平
2.可能有starvation(偏好short time job)
3.不適合用在short term scheduler(執行頻率太高，很難在極短時間內評估出精確的每個CPU burst time)

### SRTF
Def：新到達之process，其CPU burst < 目前執行中process剩的CPU time，可插隊執行

分析：
1.與SJF相比，SRTF的平均watting time、Turnaround time會較小，但content switching負擔較大
2.不公平、偏好short time job
3.會有starvation
4.preemptive

### priority
Def：挑選最高優先權的process優先取得CPU，優先權值相同則以FIFO為準

### RR
### multi level queue
### multi level feedback queue
