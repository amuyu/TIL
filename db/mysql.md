

# 일자별 쿼리

```sql
SELECT
    DATE_FORMAT(created,'%Y-%m-%d') date, COUNT(*)
FROM qa_posts 
GROUP BY
    date
ORDER BY
    date;
```

# 월별 쿼리
https://zetawiki.com/wiki/MySQL_datetime_%EC%9B%94%EB%B3%84_GROUP_BY
```sql
-- date_format 함수
SELECT DATE_FORMAT(post_date,'%Y-%m') m, COUNT(*) FROM wp_posts GROUP BY m;
-- MID 함수
SELECT MID(post_date,1,7) m, COUNT(*) FROM wp_posts GROUP BY m;
```