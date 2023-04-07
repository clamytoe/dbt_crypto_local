# Data Engineer Zoomcamp: Capstone Project (*dbt_crypto*)

> *This is the dbt portion for my capstone project as part of the DE Zoomcamp course.*

![Python version][python-version]
![Latest version][latest-version]
[![GitHub issues][issues-image]][issues-url]
[![GitHub forks][fork-image]][fork-url]
[![GitHub Stars][stars-image]][stars-url]
[![License][license-image]][license-url]

## dbt config

### Google Credentials

In order to be able to connect to your Google BigQuery account, you will need to provide your credentials. Once you have the file, save it to your local file system under `~./google/credentials/google_credentials.json`.

```bash
~ tree .google
.google
└── credentials
    └── google_credentials.json

1 directory, 1 file
```

### dbt Profile

Next you will need to create a profile in order to use with BQ. If you don't already have a `~/.dbt/profiles.yml`, create it. If you already have one, add this to the end of it:

*profiles.yml:*

```yaml
bq-dbt-decap:
  target: dev
  outputs:
    dev:
      dataset: dbt_crypto
      fixed_retries: 1
      keyfile: /.google/credentials/google_credentials.json
      location: us-central1
      method: service-account
      priority: interactive
      project: dtc-de-course-374214
      threads: 4
      timeout_seconds: 300
      type: bigquery
```

> **NOTE:** Use your own BigQuery values for `dataset`, `location`, and `project`!

### Docker Compose

Now it's time to build the image and run it.

```bash
~ docker compose build
[+] Building 21.0s (9/9) FINISHED
...
 => exporting to image                                                                                                                 1.6s
 => => exporting layers                                                                                                                1.6s
 => => writing image sha256:f954213c7281ec754f572a5c12a82a8f4f28f7bebfe5729369a503902c4af813                                           0.0s
 => => naming to docker.io/dbt/bigquery                                                                                                0.0s
```

Once the image is built, we can run a test to see if everything is configured properly:

```bash
(base) ➜  dbt_crypto_local git:(main) ✗ docker compose run dbt-bq-decap test
12:13:43  Running with dbt=1.5.0-b5
12:13:43  Found 10 models, 2 tests, 0 snapshots, 0 analyses, 355 macros, 0 operations, 0 seed files, 1 source, 0 exposures, 0 metrics, 0 groups
12:13:43
12:14:06  Concurrency: 4 threads (target='dev')
12:14:06
12:14:06  1 of 2 START test not_null_fact_coins_id ....................................... [RUN]
12:14:06  2 of 2 START test not_null_stg_coins_id ........................................ [RUN]
12:14:08  1 of 2 PASS not_null_fact_coins_id ............................................. [PASS in 2.42s]
12:14:10  2 of 2 PASS not_null_stg_coins_id .............................................. [PASS in 3.69s]
12:14:10
12:14:10  Finished running 2 tests in 0 hours 0 minutes and 26.42 seconds (26.42s).
12:14:10
12:14:10  Completed successfully
12:14:10
12:14:10  Done. PASS=2 WARN=0 ERROR=0 SKIP=0 TOTAL=2
```

To run a full test use the following command:

```bash
(base) ➜  dbt_crypto_local git:(main) ✗ docker compose run --workdir="//usr/app/dbt/" dbt-bq-decap debug
12:12:49  Running with dbt=1.5.0-b5
12:12:49  dbt version: 1.5.0-b5
12:12:49  python version: 3.11.2
12:12:49  python path: /usr/local/bin/python
12:12:49  os info: Linux-5.15.90.1-microsoft-standard-WSL2-x86_64-with-glibc2.31
12:12:49  Using profiles.yml file at /root/.dbt/profiles.yml
12:12:49  Using dbt_project.yml file at /usr/app/dbt/dbt_project.yml
12:12:49  Configuration:
12:12:50    profiles.yml file [OK found and valid]
12:12:50    dbt_project.yml file [OK found and valid]
12:12:50  Required dependencies:
12:12:50   - git [OK found]

12:12:50  Connection:
12:12:50    method: service-account
12:12:50    database: dtc-de-course-374214
12:12:50    schema: dbt_crypto
12:12:50    location: us-central1
12:12:50    priority: interactive
12:12:50    timeout_seconds: 300
12:12:50    maximum_bytes_billed: None
12:12:50    execution_project: dtc-de-course-374214
12:12:50    job_retry_deadline_seconds: None
12:12:50    job_retries: 1
12:12:50    job_creation_timeout_seconds: None
12:12:50    job_execution_timeout_seconds: 300
12:12:50    gcs_bucket: None
12:12:53    Connection test: [OK connection ok]

12:12:53  All checks passed!
```

## License

Distributed under the terms of the [MIT](https://opensource.org/licenses/MIT) license, "de_capstone" is free and open source software.

## Issues

If you encounter any problems, please [file an issue](https://github.com/clamytoe/toepack/issues) along with a detailed description.

[python-version]:https://img.shields.io/badge/python-3.10.9-brightgreen.svg
[latest-version]:https://img.shields.io/badge/version-0.1.0-blue.svg
[issues-image]:https://img.shields.io/github/issues/clamytoe/de_capstone.svg
[issues-url]:https://github.com/clamytoe/de_capstone/issues
[fork-image]:https://img.shields.io/github/forks/clamytoe/de_capstone.svg
[fork-url]:https://github.com/clamytoe/de_capstone/network
[stars-image]:https://img.shields.io/github/stars/clamytoe/de_capstone.svg
[stars-url]:https://github.com/clamytoe/de_capstone/stargazers
[license-image]:https://img.shields.io/github/license/clamytoe/de_capstone.svg
[license-url]:https://github.com/clamytoe/de_capstone/blob/master/LICENSE
