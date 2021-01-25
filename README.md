# proxysql-exporter

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/proxysql-exporter/status.svg)](https://drone.osshelp.ru/ansible/proxysql-exporter)

Role for [proxysql_exporer](https://github.com/percona/proxysql_exporter) installation and configuration. Uses the binary from [this build](https://gitea.osshelp.ru/infra/build-proxysql-exporter).

## Usage (example)

```yaml
    - role: proxysql-exporter
```

## Available parameters

### Main

| Param | Default | Description |
| -------- | -------- | -------- |
| `proxysql_exporter_setup` | `full` | Setup mode. See [OSSHelp KB article](https://oss.help/kb4895). |
| `proxysql_exporter_version` | `latest` | Which version to install. |
| `proxysql_exporter_params.data_source_name` | `stats:stats@tcp(127.0.0.1:6032)/` | Data source name. The format of this string is described [here](https://github.com/go-sql-driver/mysql#dsn-data-source-name). Check this first if getting `proxysql_up 0` in metrics. |
| `proxysql_exporter_params.listen_address` | `0.0.0.0:42004` | Address to listen on for web interface and telemetry. |
| `proxysql_exporter_params.telemetry_path` | `/metrics` | Path under which to expose metrics. |

### Collectors

Boolean params, must be set as dictionary proxysql_exporter_metrics.

| Param | Default | Description |
| -------- | -------- | -------- |
| `stats_mysql_processlist` | `true` | Collect detailed connection list from stats_mysql_processlist. |
| `mysql_connection_list` | `true` | Collect connection list from stats_mysql_processlist. |
| `mysql_connection_pool` | `true` | Collect from stats_mysql_connection_pool. |
| `mysql_status` | `true` | Collect from stats_mysql_global (SHOW MYSQL STATUS). |
| `stats_memory_metrics` | `true` | Collect memory metrics from stats_memory_metrics. |

### Misc

Usually there is no need in changing these.

| Param | Default | Description |
| -------- | -------- | -------- |
| `proxysql_exporter_config_file` | `proxysql_exporter.cfg` | Name that will be given to main configuration file. |
| `proxysql_exporter_user_name` | `proxysql_exporter` | Name of user to be created for exporter. |
| `proxysql_exporter_service_name` | `proxysql_exporter` | Name of systemd service to be created for exporter. |
| `proxysql_exporter_systemd_unit` | `/etc/systemd/system/{{proxysql_exporter_service_name}}.service` | Absolute path to systemd unit. |
| `proxysql_exporter_dirs.binary_dir` | `/usr/local/bin` | Absolute path to directory where exporter binary will be placed. |
| `proxysql_exporter_dirs.config_dir` | `/usr/local/etc/proxysql_exporter` | Absolute path to to directory where exporter configuration file will be placed. |

## FAQ

### No metrics from the exporter

Check correctness of string, given as `proxysql_exporter_params.data_source_name`. `stats:stats` could not always work and are only example. Use credentials from `monitor_username` and `monitor_password` from proxysql.cnf. Or you may be using a wrong port, recheck which port ProxySQL is listnening on.

## Useful links

- [Official documentation](https://github.com/percona/proxysql_exporter/blob/master/README.md)

## TODO

None, so far.

## License

GPL3

## Author

OSSHelp Team, see <https://oss.help>
