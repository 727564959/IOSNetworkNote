# NSURLSession

#####  -sessionWithConfiguration: delegate: delegateQueue:

session中的delegate是只读，只能由此方法在初始化时确定delegate。使用session创建task后，task的代理方法需在session的delegate中实现。

# NSURLSessionConfiguration

## 工作模式

- 一般模式（default）：工作模式类似于原来的NSURLConnection，可以使用缓存的Cache，Cookie，鉴权。
- 及时模式（ephemeral）：不使用缓存的Cache，Cookie，鉴权。
- 后台模式（background）：在后台完成上传下载，创建Configuration对象的时候需要给一个NSString的ID用于追踪完成工作的Session是哪一个。

# Delegate

`NSURLSessionDelegate` : session-level的代理方法

`NSURLSessionTaskDelegate` : task-level面向all的代理方法

`NSURLSessionDataDelegate` : task-level面向data和upload的代理方法

`NSURLSessionDownloadDelegate` : task-level的面向download的代理方法

`NSURLSessionStreamDelegate` : task-level的面向stream的代理方法

- ## NSURLSessionDelegate

##### \- (void)URLSession: didBecomeInvalidWithError:

通知>>会话已关闭

[session invalidateAndCancel]或者[session finishTasksAndInvalidate]
session被关闭时调用、持有的delegate将被清空



##### \- URLSession: didReceiveChallenge completionHandler:

询问>>服务器客户端配合验证

如果实现，则在发生连接级别身份验证质询时，将为该委托人提供向基础连接提供身份验证凭据的机会。某些类型的身份验证将应用于与服务器的给定连接上的多个请求（“ SSL服务器信任”挑战）。如果未实现此委托消息，则行为将是使用默认处理，这可能涉及用户交互。



##### - URLSessionDidFinishEventsForBackgroundURLSession:

通知>>所有后台下载任务全部完成



- ## NSURLSessionTaskDelegate


##### \- URLSession: willBeginDelayedRequest: completionHandler:

通知>>延时任务被调用



##### \- URLSession: taskIsWaitingForConnectivity:

通知>>网络受限导致任务进入等待



##### \- URLSession: task:  willPerformHTTPRedirection:response newRequest:request completionHandler:

准备开始请求、询问是否重定向



##### \- URLSession: task: didReceiveChallenge:  completionHandler:

询问>>服务器需要客户端配合验证--任务级别



##### \- URLSession: task: needNewBodyStream:

询问>>流任务的方式上传--需要客户端提供数据源



##### \- URLSession: task: didSendBodyData: totalBytesSent:  totalBytesExpectedToSend:

通知>>上传进度



##### \- URLSession: task: didFinishCollectingMetrics:

通知>>任务信息收集完成



##### \- URLSession: task: didCompleteWithError:

通知>>任务完成

# Metrics

性能统计

- ## NSURLSessionTaskTransactionMetrics



- ## NSURLSessionTaskMetrics

# NSURLCredential

# NSURLAuthenticationChallenge

# NSURLSessionAuthChallengeDisposition