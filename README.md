# pemilu2024

TLDR;

Source data is taken from [this xlsx file](https://drive.google.com/drive/folders/15nquDatlYQIM8rQMTyVwhG-PoUreE7Qr) provided by https://data-pemilu.vercel.app/.

As of 15-Feb-2024 15:49 WIB, the exported parquet file size is 85MB. It is desirable to provide diff for subsequent update cycle.

All of the generated (manually, for now) files are below:

| File Name | Created At | Info |
|---|---|---|
| ./ppwp_tps.csv.bz | 12:55 PM Feb 15 | Initial scrapping result, taken from the xlsx above |
| ./ppwp_tps.parquet | 05:28 PM Feb 15 | Initial csv -> parquet |
| ./ppwp_tps__001.parquet | 10:54 PM Feb 15 | Update 1: updated_at > 2024-02-14 20:24:41.538 |


## NOTES

Parquet files was generated using [duckdb](https://duckdb.org/), more or less like so:

```sql
--- convert the xlsx file to csv
--- e.g. in this case the exported file is 'ppwp_tps.csv'

# --- create table t0 schema
create table t0 (
    kode varchar primary key,
    provinsi_kode varchar,
    kabupaten_kota_kode varchar,
    kecamatan_kode varchar,
    kelurahan_desa_kode varchar,
    tps varchar,
    suara_paslon_1 integer,
    suara_paslon_2 integer,
    suara_paslon_3 integer,
    chasil_hal_1 varchar,
    chasil_hal_2 varchar,
    chasil_hal_3 varchar,
    suara_sah integer,
    suara_total integer,
    pemilih_dpt_j integer,
    pemilih_dpt_l integer,
    pemilih_dpt_p integer,
    pengguna_dpt_j integer,
    pengguna_dpt_l integer,
    pengguna_dpt_p integer,
    pengguna_dptb_j integer,
    pengguna_dptb_l integer,
    pengguna_dptb_p integer,
    suara_tidak_sah integer,
    pengguna_total_j integer,
    pengguna_total_l integer,
    pengguna_total_p integer,
    pengguna_non_dpt_j integer,
    pengguna_non_dpt_l integer,
    pengguna_non_dpt_p integer,
    psu varchar,
    ts timestamp,
    status_suara boolean,
    status_adm boolean,
    updated_at timestamp,
    created_at timestamp,
    url_page varchar,
    provinsi_nama varchar,
    kabupaten_kota_nama varchar,
    kecamatan_nama varchar,
    kelurahan_desa_nama varchar,
    url_api varchar
);

copy t0 from 'ppwp_tps.csv' with (header true, delimiter '\t', auto_detect false);

--- export to parquet
copy t0 to 'ppwp_tps.parquet' (format parquet);
```

Subsequent updates will be created as follow:

```sql
attach 'dbname=pemilu_2024 user=pantau password=ZubQ7IHFn1sTCP8C8rgw3T24QIiJktb8 host=vps.zakiego.com port=54325' as source_db (type postgres);

--- find the max `updated_at` from prev table
select max(updated_at) from t0;

--- create table t1 from querying source_db where updated_at > t0 max updated_at
create table t1 as select * from source_db.ppwp_tps where updated_at > (select max(updated_at) from t0);

--- export to parquet
copy t1 to 'ppwp_tps__001.parquet' (format parquet);
```

That's all for now. Contributions, both small and large, are welcome.

---

<p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><span property="dct:title">Pemilu 2024 Parquet File</span> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://github.com/reedho">Ridho</a> is marked with <a href="http://creativecommons.org/publicdomain/zero/1.0?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">CC0 1.0 Universal<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/zero.svg?ref=chooser-v1"></a></p>

