### 1. 如何过滤1min钟之内的数据？

https://blog.csdn.net/snn1410/article/details/7741283 (时间格式化样例)

```sql
select to_char(current_timestamp - interval '1 min', 'yyyyMMddhh24miss')   -- 指定时间格式
```

### 2. 如何编写地理空间函数？

http://postgis.net/workshops/postgis-intro/geometries.html

