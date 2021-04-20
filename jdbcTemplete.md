JdbcTemplete获取对象的方式

1）、查询简单类型的单个对象

```java
public <T> T queryForObject(String sql, Object[] args, Class<T> requiredType){}
Long aLong = jdbcTemplate.queryForObject(sql, new Object[]{}, Long.class);
```

2）、查询单个对象

```java
public <T> T queryForObject(String sql, Object[] args, RowMapper<T> rowMapper){}
jdbcTemplate.queryForObject(sql,new Object[]{},new BeanPropertyRowMapper<>(InvoiceInfo.class));
```

3）、查询多个对象

```java
public <T> List<T> query(String sql, Map<String, ?> paramMap, RowMapper<T> rowMapper){}
jdbcTemplate.query(sql.toString(),new Object[]{},new BeanPropertyRowMapper<>(InvoiceInfo.class))
```

