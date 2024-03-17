# End-to-End Open-Source Data Platform 

###       MELTANO     DBT     POSTGRES    


> Initialize

`mkdir <project>`

`cd <project>`

`pipx install meltano`

Create meltano project 

`meltano init <meltano_project>`

`cd meltano_project`

Install Synthea `git clone https://github.com/synthetichealth/synthea.git`
and save csv files

> Add extractors and configure

 `meltano add extractor tap-csv`

 create `/extract/config.json` and set `meltano config tap-csv set csv_files_definition extract/config.json`

 `meltano config tap-csv test`

> Add loaders and configure:

1. `meltano add loader target-jsonl`

Testing [optional]  

`meltano run tap-csv target-jsonl` will add file(s) to `/output`

2. `meltano add loader target-postgres`

```
meltano config target-postgres set host localhost
meltano config target-postgres set port 5432
meltano config target-postgres set user <user>
meltano config target-postgres set database <'database_name'>
meltano config target-postgres set password <password>
```

Testing [optional] `meltano invoke target-postgres` 

Testing [optional] `meltano run tap-csv target-postgres` will load *dev* schema with raw data

> Add transformer DBT and configure

`meltano add utility dbt-postgres`

```
meltano config dbt-postgres set host localhost
meltano config dbt-postgres set port 5432
meltano config dbt-postgres set user <user>
meltano config dbt-postgres set dbname <'database_name'>
meltano config dbt-postgres set schema <analitics_dev>
meltano config dbt-postgres set password <password>

```

>> Additional 

`meltano invoke dbt-postgres:initialize`

1. Add `packages.yml` within `/transform` and configure for tests
2. Add `/person` and within: `person.sql` and `sources.yml`
3. Add `schema.yml` within `/models`   
4. Add `macros.sql` within `/transform/macros` 

`meltano invoke dbt-postgres:build`

`meltano invoke dbt-postgres:compile`

`meltano invoke dbt-postgres:deps`

`meltano invoke dbt-postgres:run` will load *analytics_dev* schema with transformed data

`meltano invoke dbt-postgres:test`

> Run

`meltano install`

`meltano run tap-csv target-postgres dbt-postgres:run dbt-postgres:test`

