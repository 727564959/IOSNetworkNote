转载至 [iOS中流(NSStream)的使用](https://www.jianshu.com/p/522ad40f96e1)





## NSStream

流提供了一种简单的方式在不同和介质中交换数据，这种交换方式是与设备无关的。流是在通信路径中串行传输的连续的比特位序列。从编码的角度来看，流是单向的，因此流可以是输入流或输出流。除了基于文件的流外，其它形式的流都是不可查找的，这些流的数据一旦消耗完后，就无法从流对象中再次获取。

NSStream是一个抽象基类，定义了所有流对象的基础接口和属性。流是一种对缓存的抽象，对不同对象（如文件、socket和NSData）之间的数据传输提供了通用的渠道。

![stream_src_dest](/Users/zqm/Desktop/笔记/NSStream/stream_src_dest.gif)

#### 属性

流对象有一些相关的属性。大部分属性是用于处理网络安全和配置的，这些属性统称为SSL和SOCKS代理信息。

NSStreamDataWrittenToMemoryStreamKey：允许输出流查询写入到内存的数据
NSStreamFileCurrentOffsetKey：允许操作基于文件的流的读写位置

#### 代理

可以给流对象指定一个代理对象。如果没有指定，则流对象作为自己的代理。流对象调用唯一的代理方法stream:handleEvent:来处理流相关的事件。

```objective-c
//流事件
typedef NS_OPTIONS(NSUInteger, NSStreamEvent) {
    NSStreamEventNone = 0,
    NSStreamEventOpenCompleted = 1UL << 0,
    NSStreamEventHasBytesAvailable = 1UL << 1,
    NSStreamEventHasSpaceAvailable = 1UL << 2,
    NSStreamEventErrorOccurred = 1UL << 3,
    NSStreamEventEndEncountered = 1UL << 4
};
```



## NSInputStream

NSInputStream可以从文件、socket和NSData对象中获取数据。

#### 从输入流中读取数据

1、从数据源中创建和初始化一个NSInputStream实例
2、将流对象放入一个run loop中并打开流
3、处理流对象发送到其代理的事件
4、当没有更多数据可读取时，关闭并销毁流对象。



## NSOutputStream

NSOutputStream可以将数据写入文件、socket、内存缓存和NSData对象中。

#### 写入数据到输出流

1、使用要写入的数据创建和初始化一个NSOutputStream实例，并设置代理对象
2、将流对象放到run loop中并打开流
3、处理流对象发送到代理对象中的事件
4、如果流对象写入数据到内存，则通过请求NSStreamDataWrittenToMemoryStreamKey属性来获取数据
5、当没有更多数据可供写入时，处理流对象