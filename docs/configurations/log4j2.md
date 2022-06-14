# Apache Log4j 2
Log4j 2 tends to be the most recent logging framework for java.

Apache Log4j 2 is an upgrade to Log4j that provides significant improvements over its predecessor, Log4j 1.x, and provides many of the improvements available in Logback while fixing some inherent problems in Logback’s architecture.


## Configuraiton
```xml
<?xml version="1.0" encoding="UTF-8"?>;
<Configuration>
  <Properties>
    <Property name="name1">value</property>
    <Property name="name2" value="value2"/>
  </Properties>
  <filter  ... />
  <Appenders>
    <appender ... >
      <filter  ... />
    </appender>
    ...
  </Appenders>
  <Loggers>
    <Logger name="name1">
      <filter  ... />
    </Logger>
    ...
    <Root level="level">
      <AppenderRef ref="name"/>
    </Root>
  </Loggers>
</Configuration>
```

## Rolling Over
### Default Rollover Strategy

The default rollover strategy accepts both a date/time pattern and an integer from the filePattern attribute specified on the RollingFileAppender itself. If the date/time pattern is present it will be replaced with the current date and time values. If the pattern contains an integer it will be incremented on each rollover. If the pattern contains both a date/time and integer in the pattern the integer will be incremented until the result of the date/time pattern changes. If the file pattern ends with ".gz", ".zip", ".bz2", ".deflate", ".pack200", or ".xz" the resulting archive will be compressed using the compression scheme that matches the suffix

!!! note
    if the fileIndex attribute is set to "nomax" then the min and max values will be ignored and file numbering will increment by 1 and each rollover will have an incrementally higher value with no maximum number of files.
