## 原文链接

[最近学习了Http连接池](https://www.cnblogs.com/xrq730/p/10963689.html)



## 起因

业务高峰期，rpc接口响应时间超过1s；业务非高峰期，rpc接口响应时间超过600ms（正常rpc接口响应时间300ms以下）

原因是，在进行http接口调用时，每次new一个httpClient发起调用，调用时长大概在300ms+，导致接口耗时非常高。



## 线程池

为了探究“池”的影响，原文以线程池作为引子，测试用与不用“池”的一个效果差别

```java
package pool;

import org.junit.Test;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.LinkedBlockingQueue;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.atomic.AtomicLong;

/**
 * @author zzzz76
 */
public class ThreadPoolTest {
    private static final AtomicInteger FINISH_COUNT = new AtomicInteger(0);

    private static final AtomicLong COST = new AtomicLong(0);

    private static final Integer INCREASE_COUNT = 1000000;

    private static final Integer TASK_COUNT = 1000;

    @Test
    public void testRunWithoutThread() {
        // 建立任务列表
        List<Thread> tList = new ArrayList<Thread>(TASK_COUNT);

        for (int i = 0; i < TASK_COUNT; i++) {
            tList.add(new Thread(new IncreaseThread()));
        }

        // 任务并发执行
        for(Thread t: tList) {
            t.start();
        }

        for(;;);
    }

    @Test
    public void testRunWithThreadPool() {
        // 线程池配置
        ThreadPoolExecutor executor = new ThreadPoolExecutor(100, 100, 0, TimeUnit.MILLISECONDS
            , new LinkedBlockingQueue<>());

        // 任务并发执行
        for (int i = 0; i < TASK_COUNT; i++) {
            executor.submit(new IncreaseThread());
        }

        for(;;);
    }

    private class IncreaseThread implements Runnable {

        @Override
        public void run() {
            long startTime = System.currentTimeMillis();

            AtomicInteger counter = new AtomicInteger(0);
            for (int i = 0; i < INCREASE_COUNT; i++) {
                counter.incrementAndGet();
            }

            // 进行线程安全的耗时累加
            COST.addAndGet(System.currentTimeMillis() - startTime);
            if (FINISH_COUNT.incrementAndGet() == TASK_COUNT) {
                System.out.println("cost: " + COST.get() + "ms");
            }

        }
    }
}
```



结果表明，线程池使任务执行时间减少了15倍，原因有两点：

* 减少线程创建、销毁的开销
* 控制线程数量，避免对每个任务都创建线程，消耗内存



## 连接池

原文使用 httpclient 作为工具类

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.8</version>
</dependency
```

连接池基类

```java
package pool;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

/**
 * @author zzzz76
 */
public class BaseHttpClientTest {
    protected static final int REQUEST_COUNT = 5;

    protected static final String SEPERATOR = "   ";

    protected static final AtomicInteger NOW_COUNT = new AtomicInteger(0);

    protected static final StringBuilder EVERY_REQ_COST = new StringBuilder(200);

    /**
     * 获取待运行的线程
     *
     * @param runnable
     * @return
     */
    protected List<Thread> getRunThreads(Runnable runnable) {
        List<Thread> tList = new ArrayList<Thread>(REQUEST_COUNT);

        for (int i = 0; i < REQUEST_COUNT; i++) {
            tList.add(new Thread(runnable));
        }

        return tList;
    }

    /**
     * 依次启动所有线程
     *
     * @param tList
     */
    protected void startUpAllThreads(List<Thread> tList) {
        for (Thread t: tList) {
            t.start();
            try {
                Thread.sleep(300);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    protected synchronized void addCost(long cost) {
        EVERY_REQ_COST.append(cost);
        EVERY_REQ_COST.append("ms");
        EVERY_REQ_COST.append(SEPERATOR);
    }

}

```

不使用连接池测试

```java
package pool;

import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.junit.Test;

/**
 * @author zzzz76
 */
public class HttpClientWithoutPoolTest extends BaseHttpClientTest{

    @Test
    public void test() {
        startUpAllThreads(getRunThreads(new HttpThread()));
        for(;;);
    }


    private class HttpThread implements Runnable {

        @Override
        public void run() {
            CloseableHttpClient httpClient = HttpClients.custom().build();
            HttpGet httpGet = new HttpGet("https://www.baidu.com");

            long startTime = System.currentTimeMillis();
            try {
                CloseableHttpResponse response = httpClient.execute(httpGet);
                if (response != null) {
                    response.close();
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                addCost(System.currentTimeMillis() - startTime);

                if (NOW_COUNT.incrementAndGet() == REQUEST_COUNT) {
                    System.out.println(EVERY_REQ_COST.toString());
                }
            }
        }
    }
}

```



使用连接池测试

```java
package pool;

import org.apache.http.HttpHost;
import org.apache.http.client.HttpRequestRetryHandler;
import org.apache.http.client.config.RequestConfig;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.conn.routing.HttpRoute;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.impl.client.StandardHttpRequestRetryHandler;
import org.apache.http.impl.conn.PoolingHttpClientConnectionManager;
import org.junit.Before;
import org.junit.Test;

import java.util.Timer;
import java.util.TimerTask;
import java.util.concurrent.TimeUnit;

/**
 * 使用连接池测试
 *
 * @author zzzz76
 */
public class HttpClientWithPoolTest extends BaseHttpClientTest {

    private CloseableHttpClient httpClient = null;

    @Before
    public void init() {
        initHttpClient();
    }

    @Test
    public void test() {
        startUpAllThreads(getRunThreads(new HttpThread()));
        for(;;);
    }

    private class HttpThread implements Runnable {

        @Override
        public void run() {
            HttpGet httpGet = new HttpGet("https://baidu.com");
            httpGet.addHeader("Connection", "keep-alive");

            long startTime = System.currentTimeMillis();
            try {
                CloseableHttpResponse response = httpClient.execute(httpGet);
                if (response != null) {
                    response.close();
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                addCost(System.currentTimeMillis() - startTime);

                if (NOW_COUNT.incrementAndGet() == REQUEST_COUNT) {
                    System.out.println(EVERY_REQ_COST.toString());
                }
            }
        }
    }

    private void initHttpClient() {
        // 连接池管理器
        PoolingHttpClientConnectionManager connectionManager = new PoolingHttpClientConnectionManager();
        connectionManager.setMaxTotal(1);
        connectionManager.setMaxPerRoute(new HttpRoute(new HttpHost("www.baidu.com")), 1);

        // 连接配置
        RequestConfig requestConfig = RequestConfig.custom()
                .setConnectTimeout(1000)
                .setConnectionRequestTimeout(2000)
                .setSocketTimeout(3000).build();

        // 重试处理器
        HttpRequestRetryHandler retryHandler = new StandardHttpRequestRetryHandler();

        httpClient = HttpClients.custom()
                .setConnectionManager(connectionManager)
                .setDefaultRequestConfig(requestConfig)
                .setRetryHandler(retryHandler).build();

        // 定时任务 回收过时失效连接
        Timer timer = new Timer();
        timer.schedule(new TimerTask() {

            @Override
            public void run() {
                try {
                    // 关闭失效连接 并从连接池中 移除
                    connectionManager.closeExpiredConnections();
                    connectionManager.closeIdleConnections(20, TimeUnit.SECONDS);
                } catch (Throwable t) {
                    t.printStackTrace();
                }
            }
        }, 0, 1000 * 5);
    }

}

```



结果表明，除了第一次调用，后面的调用执行时间大大缩短，其性能整体提升的原因如下：

* 连接池在长短连接处做了优化，省去了大量连接建立与释放的时间



