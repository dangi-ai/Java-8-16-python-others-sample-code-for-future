<p><b>
  You can apply a null check directly inside the collector by converting the getInvoiceNum() result before collecting.

  Here are the two best ways ⬇️
  </b>
</p>

***✅ Option 1 — Handle null inline using ternary operator***

```java
orderInvoiceMap = orders.stream()
        .collect(Collectors.toMap(
                Order::getId,
                order -> order.getInvoiceNum() == null ? "NA" : order.getInvoiceNum(),
                (a, b) -> a
        ));

```

***✅ Option 2 — Using Optional.ofNullable***

```java
orderInvoiceMap = orders.stream()
        .collect(Collectors.toMap(
                Order::getId,
                order -> Optional.ofNullable(order.getInvoiceNum()).orElse("NA"),
                (a, b) -> a
        ));

```

***✅  LogBackConfig XML***

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true" scanPeriod="60 seconds">

    <!-- Common Properties -->
    <property name="APP_NAME" value="SiteManagement" />
    <property name="LOG_DIR" value="/var/log/sitemanagement" />
    
    <!-- Console Appender (Color + Throwable) -->
    <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>
                %yellow(%d{yyyy-MM-dd HH:mm:ss.SSS}) %highlight(%-5level) [%blue(%thread)] %cyan(%logger{36}) - %msg%n%ex
            </pattern>
        </encoder>
    </appender>

    <!-- Rolling File Appender (Plain Text logs) -->
    <appender name="FilePlain" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_DIR}/application.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_DIR}/application.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>7</maxHistory>     <!-- Keep 7 days -->
            <totalSizeCap>2GB</totalSizeCap>
        </rollingPolicy>
        
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n%ex</pattern>
        </encoder>
    </appender>

    <!-- JSON File Appender (For Prod Analysis Tools) -->
    <appender name="FileJson" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOG_DIR}/application-json.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_DIR}/application-json.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>7</maxHistory>
            <totalSizeCap>2GB</totalSizeCap>
        </rollingPolicy>

        <encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">
            <providers>
                <timestamp/>
                <pattern>
                    <pattern>
                        {
                            "service":"${APP_NAME}",
                            "level":"%level",
                            "thread":"%thread",
                            "logger":"%logger",
                            "message":"%msg"
                        }
                    </pattern>
                </pattern>
                <stackTrace/>
            </providers>
        </encoder>
    </appender>

    <!-- ASYNC WRAPPERS -->
    <appender name="AsyncConsole" class="ch.qos.logback.classic.AsyncAppender">
        <appender-ref ref="Console" />
        <discardingThreshold>0</discardingThreshold>
        <queueSize>10240</queueSize>
    </appender>

    <appender name="AsyncFile" class="ch.qos.logback.classic.AsyncAppender">
        <appender-ref ref="FilePlain" />
        <queueSize>20480</queueSize>
    </appender>

    <appender name="AsyncJson" class="ch.qos.logback.classic.AsyncAppender">
        <appender-ref ref="FileJson" />
        <queueSize>20480</queueSize>
    </appender>


    <!-- ***** LOGGING PER ENVIRONMENT ***** -->

    <!-- ✅ Local Profile -->
    <springProfile name="local">
        <root level="DEBUG">
            <appender-ref ref="AsyncConsole" />
        </root>
    </springProfile>

    <!-- ✅ Stage Profile -->
    <springProfile name="stage">
        <root level="INFO">
            <appender-ref ref="AsyncFile" />
            <appender-ref ref="AsyncConsole" />
        </root>
    </springProfile>

    <!-- ✅ Production Profile -->
    <springProfile name="prod">
        <root level="INFO">
            <appender-ref ref="AsyncFile" />
            <appender-ref ref="AsyncJson" />
        </root>
    </springProfile>

</configuration>


```

***✅ Dependency***
```maven
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
    <version>7.4</version>
</dependency>


```
