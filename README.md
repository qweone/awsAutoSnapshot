# awsAutoSnapshot
Take RDS &amp; EC2 snapshot 

RDS和EC2有自带的定时snapshot配置。但是设置简单。如一天只能备份一次。

事件背景：RDS在大量DML语句操作后，会新生大量的bin日志。这时如果使用restore to a point. 
将会消耗大量的时间， 因为需要等待bin日志，被单线程，逐条replay到最后一次snapshot恢复出的RDS里。
这是一个非常夸张的时间。我们遇到20G的bin日志。跑了4天，还是没能成功恢复出新的实例。
另外，这个过程其实是一直会被收费的。
为了减少恢复时间，可以做的就是及时拍摄snapshot。
所以专门写了这个脚步。放在AWS的Lambda里，通过cloudwatch的定时配置，在业务平峰时期，拍快照。并且删除历史快照

