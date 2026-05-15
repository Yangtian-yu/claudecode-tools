---
paths:
  - "**/*.java"
---
# Java Patterns

> This file extends [common/patterns.md](../common/patterns.md) with Java-specific content.

## Repository Pattern

Encapsulate data access behind an interface:

```java
public interface OrderRepository {
    Optional<Order> findById(Long id);
    List<Order> findAll();
    Order save(Order order);
    void deleteById(Long id);
}
```

Concrete implementations handle storage details (JPA, JDBC, in-memory for tests).

## 分层职责（阿里巴巴 Java 开发手册）

**SQL 必须写在 Mapper（DAO）层，禁止在 Service 层直接写 SQL：**

- Service 层只包含业务逻辑，通过 Mapper/Repository 方法调用数据访问
- 禁止在 Service 中使用 `JdbcTemplate`、`EntityManager.createNativeQuery()` 或拼接 SQL
- 所有 SQL 语句（包括动态 SQL）统一放在 Mapper XML 或 `@Query` 注解中
- Repository/DAO 接口定义数据访问契约，Service 只依赖接口方法

```java
// BAD — Service 层直接写 SQL
@Service
public class OrderService {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public List<Order> findByStatus(String status) {
        return jdbcTemplate.query("SELECT * FROM orders WHERE status = ?", mapper, status);
    }
}

// GOOD — SQL 在 Mapper/Repository 层，Service 只调用方法
@Mapper
public interface OrderMapper {
    @Select("SELECT * FROM orders WHERE status = #{status}")
    List<Order> findByStatus(@Param("status") String status);
}

@Service
public class OrderService {
    private final OrderMapper orderMapper;

    public OrderService(OrderMapper orderMapper) {
        this.orderMapper = orderMapper;
    }

    public List<Order> findByStatus(String status) {
        return orderMapper.findByStatus(status);
    }
}
```

## Service Layer

Business logic in service classes; keep controllers and repositories thin:

```java
public class OrderService {
    private final OrderRepository orderRepository;
    private final PaymentGateway paymentGateway;

    public OrderService(OrderRepository orderRepository, PaymentGateway paymentGateway) {
        this.orderRepository = orderRepository;
        this.paymentGateway = paymentGateway;
    }

    public OrderSummary placeOrder(CreateOrderRequest request) {
        var order = Order.from(request);
        paymentGateway.charge(order.total());
        var saved = orderRepository.save(order);
        return OrderSummary.from(saved);
    }
}
```

## Constructor Injection

Always use constructor injection — never field injection:

```java
// GOOD — constructor injection (testable, immutable)
public class NotificationService {
    private final EmailSender emailSender;

    public NotificationService(EmailSender emailSender) {
        this.emailSender = emailSender;
    }
}

// BAD — field injection (untestable without reflection, requires framework magic)
public class NotificationService {
    @Inject // or @Autowired
    private EmailSender emailSender;
}
```

## DTO Mapping

Use records for DTOs. Map at service/controller boundaries:

```java
public record OrderResponse(Long id, String customer, BigDecimal total) {
    public static OrderResponse from(Order order) {
        return new OrderResponse(order.getId(), order.getCustomerName(), order.getTotal());
    }
}
```

## Builder Pattern

Use for objects with many optional parameters:

```java
public class SearchCriteria {
    private final String query;
    private final int page;
    private final int size;
    private final String sortBy;

    private SearchCriteria(Builder builder) {
        this.query = builder.query;
        this.page = builder.page;
        this.size = builder.size;
        this.sortBy = builder.sortBy;
    }

    public static class Builder {
        private String query = "";
        private int page = 0;
        private int size = 20;
        private String sortBy = "id";

        public Builder query(String query) { this.query = query; return this; }
        public Builder page(int page) { this.page = page; return this; }
        public Builder size(int size) { this.size = size; return this; }
        public Builder sortBy(String sortBy) { this.sortBy = sortBy; return this; }
        public SearchCriteria build() { return new SearchCriteria(this); }
    }
}
```

## Sealed Types for Domain Models

```java
public sealed interface PaymentResult permits PaymentSuccess, PaymentFailure {
    record PaymentSuccess(String transactionId, BigDecimal amount) implements PaymentResult {}
    record PaymentFailure(String errorCode, String message) implements PaymentResult {}
}

// Exhaustive handling (Java 21+)
String message = switch (result) {
    case PaymentSuccess s -> "Paid: " + s.transactionId();
    case PaymentFailure f -> "Failed: " + f.errorCode();
};
```

## API Response Envelope

Consistent API responses:

```java
public record ApiResponse<T>(boolean success, T data, String error) {
    public static <T> ApiResponse<T> ok(T data) {
        return new ApiResponse<>(true, data, null);
    }
    public static <T> ApiResponse<T> error(String message) {
        return new ApiResponse<>(false, null, message);
    }
}
```

## 后端数据库表规范

### Schema 分离

数据库采用 Schema 分离策略，**禁止使用裸表名或 JdbcTemplate 裸写 SQL**：

| Schema | 用途 | 示例 |
|--------|------|------|
| `sys` | 系统表（基建框架） | `sys.t_employee`、`sys.t_role`、`sys.t_file` |
| `biz` | 业务表（业务模块） | `biz.t_rfid_tag`、`biz.t_trace_record`、`biz.t_station` |

### Entity 必须声明完整 Schema

```java
@TableName("sys.t_file")     // 系统表
@TableName("biz.t_rfid_tag") // 业务表
```

### 规则

- 所有 `@TableName` 注解必须包含 Schema 前缀（`sys.` 或 `biz.`）
- Mapper XML 中的表名同样必须带 Schema：`SELECT * FROM biz.t_rfid_tag`
- 禁止使用裸表名（如 `t_rfid_tag`），防止跨 Schema 查询错误
- 新建表时根据用途选择 Schema：系统级 → `sys`，业务级 → `biz`

```java
// BAD — 裸表名，不知道属于哪个 Schema
@TableName("t_rfid_tag")

// GOOD — 明确 Schema 归属
@TableName("biz.t_rfid_tag")
```

## References

See skill: `springboot-patterns` for Spring Boot architecture patterns.
See skill: `jpa-patterns` for entity design and query optimization.
