# PostgreSQLのテーブルをTRUNCATEしたいときに気をつけること

## 結論

TRUNCATE table後、idもクリアしたい場合は
SERIAL型で設定したPRIMARY KEYの値をクリアしないといけない。

## 操作

例えば、以下のテーブルが作られているとする

```sql
CREATE TABLE customer (
  id         SERIAL PRIMARY KEY,
  name       TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP,
);
```

で、100レコードぐらいINSERTする

```sql
INSERT INTO customer
  (name, created_at, update_at)
  VALUES ('bob', '2015-02-03 10:00:00', '2015-02-03 10:00:00');

INSERT INTO customer
  (name, created_at, update_at)
  VALUES ('tom', '2015-02-03 10:00:00', '2015-02-03 10:00:00');

INSERT INTO customer
  (name, created_at, update_at)
  VALUES ('hey', '2015-02-03 10:00:00', '2015-02-03 10:00:00');
```

TRUNCATEコマンドで、テーブルをクリアする

```sql
TRUNCATE customer;
```

改めてINSERTを行うと、idが101から開始してしまう

```sql
INSERT INTO customer
  (name, created_at, update_at)
  VALUES ('hey', '2015-02-03 10:00:00', '2015-02-03 10:00:00');

SELECT * FROM customer;
> 101 | hey | 2015-02-03 10:00:00 | 2015-02-03 10:00:00 |
```

ALTER SEQUENCE文で、SERIAL型のPRIMARY KEYの値をリセットする。

```sql
ALTER SEQUENCE customer_id_seq RESTART WITH 1;
```

以上でOK。
