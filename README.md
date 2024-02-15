# pemilu2024

TLDR;

Source data is taken from [here](https://drive.google.com/drive/folders/15nquDatlYQIM8rQMTyVwhG-PoUreE7Qr) of https://data-pemilu.vercel.app/.

Parquet file was generated more or less like so:

```
# --- convert xls file to csv
...

# --- import to duckdb:

create table t1 (
    kode varchar, 
    provinsi_kode varchar, 
    kabupaten_kota_kode varchar,
    kecamatan_kode varchar,
    kelurahan_desa_kode varchar,
    tps varchar,
    suara_paslon_1 bigint,
    suara_paslon_2 bigint,
    suara_paslon_3 bigint,
    chasil_hal_1 varchar,
    chasil_hal_2 varchar,
    chasil_hal_3 varchar,
    suara_sah bigint,
    suara_total bigint,
    pemilih_dpt_j bigint,
    pemilih_dpt_l bigint,
    pemilih_dpt_p bigint,
    pengguna_dpt_j bigint,
    pengguna_dpt_l bigint,
    pengguna_dpt_p bigint,
    pengguna_dptb_j bigint,
    pengguna_dptb_l bigint,
    pengguna_dptb_p bigint,
    suara_tidak_sah bigint,
    pengguna_total_j bigint,
    pengguna_total_l bigint,
    pengguna_total_p bigint,
    pengguna_non_dpt_j bigint,
    pengguna_non_dpt_l bigint,
    pengguna_non_dpt_p bigint,
    psu bigint,
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

copy t1 from 'ppwp_tps.csv' with (header true, delimiter '\t', auto_detect false);

# --- export to parquet

copy t1 to 'ppwp_tps.parquet' (format parquet);
```

