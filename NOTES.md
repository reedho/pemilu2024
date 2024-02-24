# Notes

## Sat Feb 24 2024

```sql
attach 'dbname=pantau2024 user=ropantau password=WW0xV00xZ3pRbWhqTTA0ellq host=103.76.121.53 port=54321' as source_db (type postgres);

create table ppwp_tps__t0_0 as select * from source_db.ppwp_tps limit 300000;
create table ppwp_tps__t0_1 as select * from source_db.ppwp_tps offset 300000 limit 300000;
create table ppwp_tps__t0_2 as select * from source_db.ppwp_tps offset 600000 limit 300000;


copy ppwp_tps__t0_0 to 'ppwp_tps__t0_0.parquet' (format parquet);
copy ppwp_tps__t0_1 to 'ppwp_tps__t0_1.parquet' (format parquet);
copy ppwp_tps__t0_2 to 'ppwp_tps__t0_2.parquet' (format parquet);
```


