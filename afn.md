## AFURLSessionManager

##### \-  initWithSessionConfiguration:

​	AFHTTPSessionManager的全能初始化方法。

##### \- dataTaskWithRequest:uploadProgress:downloadProgress: completionHandler:

​	生成task对象，并为其同步生成一个中间代理对象，通过mutableTaskDelegatesKeyedByTaskIdentifier字典保存。运用策略模式，在代理通知是调用对应的方法。



## AFHTTPSessionManager

##### \- dataTaskWithHTTPMethod: URLString: parameters: headers: uploadProgress: downloadProgress : success: failure: 

​	AFHTTPSessionManager发送请求的全能方法。

步骤：

1. 通过AFHTTPRequestSerializer对象requestWithMethod: URLString: parameters: error:方法生成NSMutableURLRequest对象，为request对象添加头部。
2. 通过dataTaskWithRequest: uploadProgress:downloadProgress:completionHandler:方法生成一个NSURLSessionDataTask对象，并返回该对象。

## AFURLSessionManagerTaskDelegate

用来处理所有task代理的对象。

对返回的头部(NSURLResponse)与数据(NSData)进行解析,生成一个responseObject字典。

Manager处理代理时先调用本类代理函数，然后调用manager本身的block。

- mutableData: 接收数据的缓存。

- AFNetworkingTaskDidCompleteNotification

