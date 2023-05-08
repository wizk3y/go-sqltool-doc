# Prepare opt
[Back to Home](https://github.com/wizk3y/go-sqltool)

---
Sometimes struct having more than necessary field, or field with special value like `time.Time`, you might need something like SQLToolOpt.

## Opt
- `SerialColumnOpt`: define serial/auto increment column of table, which is ignore when execute insert command. Default: `id`.
- `NullableColumnsOpt`: define list of column when execute insert/update command is null/nil-able, separate by comma.
- `DateTimeColumnsOpt`: define list of column that need convert from/to `time.Time` when execute command or scan response value.
- `AutoCreateDateTimeColumnsOpt`: define list of column is auto-fill current time if value nil when execute insert command.
- `AutoUpdateDateTimeColumnsOpt`: define list of column is auto-fill current time if value nil when execute insert/update command.
- `AllowColumnsOpt`: define list of column that need scan type and value. This will help you control `Prepare` method better without duplicate define struct. This opt has higher priority than `IgnoreColumnsOpt`.
- `IgnoreColumnsOpt`: define list of column that no need scan type and value. This will help you control `Prepare` method better without duplicate define struct.


---
[Back to Home](https://github.com/wizk3y/go-sqltool)