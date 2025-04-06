
# 📒 Spring Boot Logging (SLF4J + Logback)

> Logging is essential for monitoring applications, diagnosing issues, and debugging. Spring Boot provides first-class support for logging via **SLF4J** with **Logback** as the default implementation.

---

## 🧠 What is Logging?

Logging is the process of recording information about a program’s execution. It helps:
- Monitor application health
- Diagnose bugs
- Audit actions and flow
- Capture and store runtime behavior

---

## 📦 Logging Frameworks in Java

Several logging frameworks exist:
- **Log4j**
- **Logback**
- **TinyLog**
- **java.util.logging (JUL)**

Spring Boot **uses SLF4J** as a facade (abstraction) to unify all of these.

> **SLF4J** = Simple Logging Facade for Java  
It allows switching the underlying logging framework with minimal configuration.

---

## 🧩 Components of a Logging Framework

Each logging framework typically consists of:

| Component | Description |
|----------|-------------|
| **Logger** | Captures the log messages |
| **Formatter (Layout)** | Formats how the log message looks |
| **Handler (Appender)** | Sends log output to destination (Console, File, etc.) |

---

## 🪜 Log Levels

Spring Boot supports the following log levels:

| Level | Description |
|-------|-------------|
| `TRACE` | Most detailed info (method calls, variables, etc.) |
| `DEBUG` | Detailed info for debugging |
| `INFO` | General runtime events (e.g., startup completed) |
| `WARN` | Potential problems or warnings |
| `ERROR` | Runtime errors that don't crash the system |
| `FATAL` | Severe errors that likely crash the system *(rare in Spring Boot)* |

> **Hierarchy**:  
TRACE < DEBUG < INFO < WARN < ERROR < FATAL

Setting a log level enables that level **and all more severe ones**.

---

## ⚙️ Setting Log Levels in Spring Boot

Configure log levels in `application.properties` or `application.yml`:

```properties
# Global level
logging.level.root=INFO

# Package-specific level
logging.level.com.example.myapp=DEBUG
```

```yaml
# application.yml example
logging:
  level:
    root: INFO
    com.example.myapp: DEBUG
```

---

## 🎨 Log Message Format

Customize the log message format using format patterns.

```properties
# Console output pattern
logging.pattern.console=%d [%level] %c{1.} [%t] %m%n
```

| Pattern | Description |
|---------|-------------|
| `%d` | Date/Time |
| `%level` | Log level (INFO, DEBUG, etc.) |
| `%c{1.}` | Class name (shortened) |
| `%t` | Thread name |
| `%m` | The log message |
| `%n` | Newline |

### Example Output:
```
2025-04-06 12:00:00 [INFO] MyController [main] Starting app...
```

---

## 📂 Logging to File

Log files can be configured using:

```properties
# Set log file name
logging.file.name=error.log

# File output format
logging.pattern.file=%clr(%d{yyyy-MM-dd HH:mm:ss.SSS}){green} [%level] %c{1.} [%t] %m%n
```

> `%clr(...)` = Adds ANSI colors (works in supporting consoles)

---

## 🛠️ Logback Configuration (Optional)

Spring Boot uses **Logback** by default. Advanced configuration can be done via `logback-spring.xml`:

```xml
<configuration>
  <appender name="FILE" class="ch.qos.logback.core.FileAppender">
    <file>myapp.log</file>
    <encoder>
      <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    </encoder>
  </appender>

  <root level="INFO">
    <appender-ref ref="FILE" />
  </root>
</configuration>
```

> Place `logback-spring.xml` under `src/main/resources`.

---

## 🧪 Logging in Code (SLF4J API)

Use SLF4J annotations or manual instantiation.

### 🟡 Example with Lombok:
```java
import lombok.extern.slf4j.Slf4j;

@Slf4j
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String sayHello() {
        log.info("Hello endpoint was called");
        return "Hello!";
    }
}
```

### 🔵 Without Lombok:
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@RestController
public class HelloController {
    private static final Logger logger = LoggerFactory.getLogger(HelloController.class);

    @GetMapping("/hello")
    public String sayHello() {
        logger.debug("Inside sayHello()");
        return "Hello!";
    }
}
```

---

## ✅ Best Practices

- Use `INFO` and `WARN` for important business events
- Use `DEBUG` and `TRACE` during development
- Avoid excessive logging in production
- Do not log sensitive data (passwords, tokens, etc.)
- Externalize logging configs for flexibility
- Use async appenders for high-performance applications

---

## 📚 References

- [Spring Boot Logging Docs](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.logging)
- [SLF4J Manual](http://www.slf4j.org/manual.html)
- [Logback Documentation](https://logback.qos.ch/manual/)

---

💡 **Pro Tip**: To color logs in the terminal or use JSON logs, explore `logstash-logback-encoder`.
