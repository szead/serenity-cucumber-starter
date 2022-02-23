# How to reproduce the issue

 - Make sure the template tests are running okay with running:
```shell
$ gradle clean test -Pdriver=chrome
```
 - Start Docker Desktop for Mac (or stop selenium-hub related docker containers)

Run this command copied from docker-selenium github: 
```shell
$ docker run -d -p 4444:4444 -p 7900:7900 --shm-size="2g" selenium/standalone-chrome:4.1.2-20220217
```
Make sure docker chrome is up and running by opening `http://localhost:7900` no vnc client.
Password is "secret".

Replace the webdriver part in the `/src/test/resources/serenity.conf` file with the following:

```
webdriver {
  driver = remote
  remote {
    url="http://localhost:4444"
    driver=chrome
  }
}
```

Try to run the tests again with:
```shell
$ gradle clean test -Pdriver=chrome
```

Expect to get this error:
```java
starter.CucumberTestSuite > Search by keyword.Searching for a term STANDARD_ERROR
    Feb 23, 2022 5:01:28 PM org.openqa.selenium.remote.tracing.opentelemetry.OpenTelemetryTracer createTracer
    INFO: Using OpenTelemetry for tracing
    Feb 23, 2022 5:04:02 PM org.openqa.selenium.remote.ProtocolHandshake createSession
    INFO: Detected dialect: W3C
    Feb 23, 2022 5:04:02 PM org.openqa.selenium.devtools.CdpVersionFinder findNearestMatch
    WARNING: Unable to find an exact match for CDP version 98, so returning the closest version found: 97
    Feb 23, 2022 5:04:02 PM org.openqa.selenium.devtools.CdpVersionFinder findNearestMatch
    INFO: Found CDP implementation for version 98 of 97
    Feb 23, 2022 5:04:12 PM org.openqa.selenium.remote.http.netty.NettyWebSocket lambda$new$0
    WARNING: connection timed out: /172.17.0.4:4444
    java.net.ConnectException: connection timed out: /172.17.0.4:4444
        at org.asynchttpclient.netty.channel.NettyConnectListener.onFailure(NettyConnectListener.java:179)
        at org.asynchttpclient.netty.channel.NettyChannelConnector$1.onFailure(NettyChannelConnector.java:108)
        at org.asynchttpclient.netty.SimpleChannelFutureListener.operationComplete(SimpleChannelFutureListener.java:28)
        at org.asynchttpclient.netty.SimpleChannelFutureListener.operationComplete(SimpleChannelFutureListener.java:20)
        at io.netty.util.concurrent.DefaultPromise.notifyListener0(DefaultPromise.java:578)
        at io.netty.util.concurrent.DefaultPromise.notifyListeners0(DefaultPromise.java:571)
        at io.netty.util.concurrent.DefaultPromise.notifyListenersNow(DefaultPromise.java:550)
        at io.netty.util.concurrent.DefaultPromise.notifyListeners(DefaultPromise.java:491)
        at io.netty.util.concurrent.DefaultPromise.setValue0(DefaultPromise.java:616)
        at io.netty.util.concurrent.DefaultPromise.setFailure0(DefaultPromise.java:609)
        at io.netty.util.concurrent.DefaultPromise.tryFailure(DefaultPromise.java:117)
        at io.netty.channel.nio.AbstractNioChannel$AbstractNioUnsafe$1.run(AbstractNioChannel.java:262)
        at io.netty.util.concurrent.PromiseTask.runTask(PromiseTask.java:98)
        at io.netty.util.concurrent.ScheduledFutureTask.run(ScheduledFutureTask.java:170)
        at io.netty.util.concurrent.AbstractEventExecutor.safeExecute(AbstractEventExecutor.java:164)
        at io.netty.util.concurrent.SingleThreadEventExecutor.runAllTasks(SingleThreadEventExecutor.java:469)
        at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:503)
        at io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:986)
        at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
        at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
        at java.base/java.lang.Thread.run(Thread.java:833)
    Caused by: io.netty.channel.ConnectTimeoutException: connection timed out: /172.17.0.4:4444
        at io.netty.channel.nio.AbstractNioChannel$AbstractNioUnsafe$1.run(AbstractNioChannel.java:261)
        ... 9 more

    Feb 23, 2022 5:04:12 PM org.openqa.selenium.remote.http.WebSocket$Listener onError
    WARNING: connection timed out: /172.17.0.4:4444
    java.net.ConnectException: connection timed out: /172.17.0.4:4444
        at org.asynchttpclient.netty.channel.NettyConnectListener.onFailure(NettyConnectListener.java:179)
        at org.asynchttpclient.netty.channel.NettyChannelConnector$1.onFailure(NettyChannelConnector.java:108)
        at org.asynchttpclient.netty.SimpleChannelFutureListener.operationComplete(SimpleChannelFutureListener.java:28)
        at org.asynchttpclient.netty.SimpleChannelFutureListener.operationComplete(SimpleChannelFutureListener.java:20)
        at io.netty.util.concurrent.DefaultPromise.notifyListener0(DefaultPromise.java:578)
        at io.netty.util.concurrent.DefaultPromise.notifyListeners0(DefaultPromise.java:571)
        at io.netty.util.concurrent.DefaultPromise.notifyListenersNow(DefaultPromise.java:550)
        at io.netty.util.concurrent.DefaultPromise.notifyListeners(DefaultPromise.java:491)
        at io.netty.util.concurrent.DefaultPromise.setValue0(DefaultPromise.java:616)
        at io.netty.util.concurrent.DefaultPromise.setFailure0(DefaultPromise.java:609)
        at io.netty.util.concurrent.DefaultPromise.tryFailure(DefaultPromise.java:117)
        at io.netty.channel.nio.AbstractNioChannel$AbstractNioUnsafe$1.run(AbstractNioChannel.java:262)
        at io.netty.util.concurrent.PromiseTask.runTask(PromiseTask.java:98)
        at io.netty.util.concurrent.ScheduledFutureTask.run(ScheduledFutureTask.java:170)
        at io.netty.util.concurrent.AbstractEventExecutor.safeExecute(AbstractEventExecutor.java:164)
        at io.netty.util.concurrent.SingleThreadEventExecutor.runAllTasks(SingleThreadEventExecutor.java:469)
        at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:503)
        at io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:986)
        at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
        at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
        at java.base/java.lang.Thread.run(Thread.java:833)
    Caused by: io.netty.channel.ConnectTimeoutException: connection timed out: /172.17.0.4:4444
        at io.netty.channel.nio.AbstractNioChannel$AbstractNioUnsafe$1.run(AbstractNioChannel.java:261)
        ... 9 more

```
Now if you change back Serenity version in `build.gradle` to

```groovy
    serenityCoreVersion = '3.0.5'
    serenityCucumberVersion = '3.0.5'
```

And rebuild, rerun the tests will work with no error.
