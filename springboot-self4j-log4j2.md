### 1.引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <!-- 去除默认的 Logback 日志框架依赖 springboot 默认的是self4j + logback-->
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<!-- Log4j2 日志框架 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

 ### 2. 编写log4j2.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->
<!--Configuration后面的status，这个用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，你会看到log4j2内部各种详细输出-->
<!--monitorInterval：Log4j能够自动检测修改配置 文件和重新配置本身，设置间隔秒数-->
<configuration status="INFO" monitorInterval="1800">
    <Properties>
        <!-- ===========================================公共配置=========================================== -->
        <!-- 设置日志文件目录的名称 -->
        <property name="logFileName">logs</property>
        <!-- 日志默认存放的位置,可以设置为项目根路径下,也可指定绝对路径 -->
        <!-- 存放路径一:通用路径,window平台 -->
        <!-- <property name="basePath">E:/project/RainCloud/server/${logFileName}</property> -->
        <!-- 存放路径二:web工程专用,java项目没有这个变量,需要删掉,否则会报异常,这里把日志放在web项目的根目录下 -->
        <property name="basePath">${logFileName}</property>

        <!-- 控制台默认输出格式,"%-5level":日志级别,"%l":输出完整的错误位置,是小写的L,因为有行号显示,所以影响日志输出的性能 -->
        <property name="console_log_pattern">%d{yyyy-MM-dd HH:mm:ss.SSS} [%-5level] %l - %m%n</property>
        <!-- 控制台显示的日志最低级别 -->
        <property name="console_print_level">DEBUG</property>
        <!-- 日志文件输出格式：%C:大写,类名;%M:方法名;%L:行号;%m:错误信息;%n:换行 -->
        <property name="log_pattern">%d{yyyy-MM-dd HH:mm:ss.SSS} [%-5level] %C.%M[%L line] - %m%n</property>
        <!-- 日志默认输出级别 -->
        <property name="output_log_level">INFO</property>


        <!-- 日志默认切割的最小单位 -->
        <property name="every_file_size">100MB</property>
        <!-- 日志默认输出级别 -->
        <property name="output_log_level">DEBUG</property>

        <!-- ===========================================所有级别日志配置=========================================== -->
        <!-- 日志默认存放路径(所有级别日志) -->
        <property name="all_fileName">${basePath}/all.log</property>
        <!-- 日志默认压缩路径,将超过指定文件大小的日志,自动存入按"年月"建立的文件夹下面并进行压缩,作为存档 -->
        <property name="all_filePattern">${basePath}/%d{yyyy-MM}/all-%d{yyyy-MM-dd}-%i.log.gz</property>
        <!-- 日志默认同类型日志,同一文件夹下可以存放的数量,不设置此属性则默认为7个,filePattern最后要带%i才会生效 -->
        <property name="all_max">7</property>
        <!-- 日志默认同类型日志,多久生成一个新的日志文件,这个配置需要和filePattern结合使用;
                如果设置为1,filePattern是%d{yyyy-MM-dd}到天的格式,则间隔一天生成一个文件
                如果设置为12,filePattern是%d{yyyy-MM-dd-HH}到小时的格式,则间隔12小时生成一个文件 -->
        <property name="all_timeInterval">1</property>
        <!-- 日志默认同类型日志,是否对封存时间进行调制,若为true,则封存时间将以0点为边界进行调整,
                如:现在是早上3am,interval是4,那么第一次滚动是在4am,接着是8am,12am...而不是7am -->
        <property name="all_timeModulate">true</property>

        <!-- ============================================Info级别日志============================================ -->
        <!-- Info日志默认存放路径(Info级别日志) -->
        <property name="info_fileName">${basePath}/info.log</property>
        <!-- Info日志默认压缩路径,将超过指定文件大小的日志,自动存入按"年月"建立的文件夹下面并进行压缩,作为存档 -->
        <property name="info_filePattern">${basePath}/%d{yyyy-MM}/info-%d{yyyy-MM-dd}-%i.log.gz</property>
        <!-- Info日志默认同一文件夹下可以存放的数量,不设置此属性则默认为7个 -->
        <property name="info_max">7</property>
        <!-- 日志默认同类型日志,多久生成一个新的日志文件,这个配置需要和filePattern结合使用;
                如果设置为1,filePattern是%d{yyyy-MM-dd}到天的格式,则间隔一天生成一个文件
                如果设置为12,filePattern是%d{yyyy-MM-dd-HH}到小时的格式,则间隔12小时生成一个文件 -->
        <property name="info_timeInterval">1</property>
        <!-- 日志默认同类型日志,是否对封存时间进行调制,若为true,则封存时间将以0点为边界进行调整,
                如:现在是早上3am,interval是4,那么第一次滚动是在4am,接着是8am,12am...而不是7am -->
        <property name="info_timeModulate">true</property>

        <!-- ============================================Warn级别日志============================================ -->
        <!-- Warn日志默认存放路径(Warn级别日志) -->
        <property name="warn_fileName">${basePath}/warn.log</property>
        <!-- Warn日志默认压缩路径,将超过指定文件大小的日志,自动存入按"年月"建立的文件夹下面并进行压缩,作为存档 -->
        <property name="warn_filePattern">${basePath}/%d{yyyy-MM}/warn-%d{yyyy-MM-dd}-%i.log.gz</property>
        <!-- Warn日志默认同一文件夹下可以存放的数量,不设置此属性则默认为7个 -->
        <property name="warn_max">7</property>
        <!-- 日志默认同类型日志,多久生成一个新的日志文件,这个配置需要和filePattern结合使用;
                如果设置为1,filePattern是%d{yyyy-MM-dd}到天的格式,则间隔一天生成一个文件
                如果设置为12,filePattern是%d{yyyy-MM-dd-HH}到小时的格式,则间隔12小时生成一个文件 -->
        <property name="warn_timeInterval">1</property>
        <!-- 日志默认同类型日志,是否对封存时间进行调制,若为true,则封存时间将以0点为边界进行调整,
                如:现在是早上3am,interval是4,那么第一次滚动是在4am,接着是8am,12am...而不是7am -->
        <property name="warn_timeModulate">true</property>

        <!-- ============================================Error级别日志============================================ -->
        <!-- Error日志默认存放路径(Error级别日志) -->
        <property name="error_fileName">${basePath}/error.log</property>
        <!-- Error日志默认压缩路径,将超过指定文件大小的日志,自动存入按"年月"建立的文件夹下面并进行压缩,作为存档 -->
        <property name="error_filePattern">${basePath}/%d{yyyy-MM}/error-%d{yyyy-MM-dd}-%i.log.gz</property>
        <!-- Error日志默认同一文件夹下可以存放的数量,不设置此属性则默认为7个 -->
        <property name="error_max">7</property>
        <!-- 日志默认同类型日志,多久生成一个新的日志文件,这个配置需要和filePattern结合使用;
                如果设置为1,filePattern是%d{yyyy-MM-dd}到天的格式,则间隔一天生成一个文件
                如果设置为12,filePattern是%d{yyyy-MM-dd-HH}到小时的格式,则间隔12小时生成一个文件 -->
        <property name="error_timeInterval">1</property>
        <!-- 日志默认同类型日志,是否对封存时间进行调制,若为true,则封存时间将以0点为边界进行调整,
                如:现在是早上3am,interval是4,那么第一次滚动是在4am,接着是8am,12am...而不是7am -->
        <property name="error_timeModulate">true</property>
    </Properties>

    <appenders>
        <!-- =======================================用来定义输出到控制台的配置======================================= -->
        <Console name="Console" target="SYSTEM_OUT">
            <!-- 设置控制台只输出level及以上级别的信息(onMatch),其他的直接拒绝(onMismatch)-->
            <ThresholdFilter level="${console_print_level}" onMatch="ACCEPT" onMismatch="DENY" />
            <!-- 设置输出格式,不设置默认为:%m%n -->
            <PatternLayout pattern="${console_log_pattern}" />
        </Console>

        <!-- =======================================打印所有级别的日志到文件======================================= -->
        <RollingFile name="AllFile" fileName="${all_fileName}" filePattern="${all_filePattern}">
            <PatternLayout pattern="${log_pattern}" />
            <Policies>
                <TimeBasedTriggeringPolicy interval="${all_timeInterval}" modulate="${all_timeModulate}" />
                <SizeBasedTriggeringPolicy size="${every_file_size}" />
            </Policies>
            <!-- 设置同类型日志,同一文件夹下可以存放的数量,如果不设置此属性则默认存放7个文件 -->
            <DefaultRolloverStrategy max="${all_max}" />
        </RollingFile>

        <!-- =======================================打印INFO级别的日志到文件======================================= -->
        <RollingFile name="InfoFile" fileName="${info_fileName}" filePattern="${info_filePattern}">
            <PatternLayout pattern="${log_pattern}" />
            <Policies>
                <TimeBasedTriggeringPolicy interval="${info_timeInterval}" modulate="${info_timeModulate}" />
                <SizeBasedTriggeringPolicy size="${every_file_size}" />
            </Policies>
            <DefaultRolloverStrategy max="${info_max}" />
            <Filters>
                <ThresholdFilter level="WARN" onMatch="DENY" onMismatch="NEUTRAL" />
                <ThresholdFilter level="INFO" onMatch="ACCEPT" onMismatch="DENY" />
            </Filters>
        </RollingFile>

        <!-- =======================================打印WARN级别的日志到文件======================================= -->
        <RollingFile name="WarnFile" fileName="${warn_fileName}" filePattern="${warn_filePattern}">
            <PatternLayout pattern="${log_pattern}" />
            <Policies>
                <TimeBasedTriggeringPolicy interval="${warn_timeInterval}" modulate="${warn_timeModulate}" />
                <SizeBasedTriggeringPolicy size="${every_file_size}" />
            </Policies>
            <DefaultRolloverStrategy max="${warn_max}" />
            <Filters>
                <ThresholdFilter level="ERROR" onMatch="DENY" onMismatch="NEUTRAL" />
                <ThresholdFilter level="WARN" onMatch="ACCEPT" onMismatch="DENY" />
            </Filters>
        </RollingFile>

        <!-- =======================================打印ERROR级别的日志到文件======================================= -->
        <RollingFile name="ErrorFile" fileName="${error_fileName}" filePattern="${error_filePattern}">
            <PatternLayout pattern="${log_pattern}" />
            <Policies>
                <TimeBasedTriggeringPolicy interval="${error_timeInterval}" modulate="${error_timeModulate}" />
                <SizeBasedTriggeringPolicy size="${every_file_size}" />
            </Policies>
            <DefaultRolloverStrategy max="${error_max}" />
            <Filters>
                <ThresholdFilter level="FATAL" onMatch="DENY" onMismatch="NEUTRAL" />
                <ThresholdFilter level="ERROR" onMatch="ACCEPT" onMismatch="DENY" />
            </Filters>
        </RollingFile>

    </appenders>

    <!--定义logger,只有定义了logger并引入的appender,appender才会生效-->
    <loggers>
        <!-- 设置java.sql包下的日志只打印DEBUG及以上级别的日志,此设置可以支持sql语句的日志打印 -->
        <logger name="java.sql" level="DEBUG" additivity="false">
            <appender-ref ref="Console" />
        </logger>
        <!-- 设置org.mybatis.spring包下的日志只打印WARN及以上级别的日志 -->
        <logger name="org.mybatis.spring" level="WARN" additivity="false">
            <appender-ref ref="Console" />
        </logger>
        <!-- 设置org.springframework包下的日志只打印WARN及以上级别的日志 -->
        <logger name="org.springframework" level="WARN" additivity="false">
            <appender-ref ref="Console" />
        </logger>

        <!--建立一个默认的root的logger-->
        <root level="${output_log_level}">
            <appender-ref ref="AllFile" />
            <appender-ref ref="Console" />
            <appender-ref ref="InfoFile" />
            <appender-ref ref="WarnFile" />
            <appender-ref ref="ErrorFile" />
        </root>
    </loggers>
</configuration>
```