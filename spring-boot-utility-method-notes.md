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
