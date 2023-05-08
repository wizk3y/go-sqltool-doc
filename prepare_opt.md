# Prepare opt
[Back to Home](https://github.com/wizk3y/go-sqltool)

---
Sometimes struct having more than necessary field, or field with special value like `time.Time`, you might need something like SQLToolOpt. Below is an example using with [squirrel](https://github.com/Masterminds/squirrel).

```go
type User struct {
	ID        int64  `json:"id"`
	CreatedAt int64  `json:"created_at"`
	UpdatedAt int64  `json:"updated_at"`
	Name      string `json:"name"`
	Email     string `json:"email"`
}

func QueryUser(ctx context.Context, db *sql.DB) (*User, error) {
	user := User{}

	st := sqltool.NewTool(ctx, db)
	st.PrepareSelect(&user,
		sqltool.DateTimeColumnsOpt([]string{"created_at", "updated_at"}), // created_at, updated_at columns has database type is datetime and need convert to timestamp in int64
		sqltool.IgnoreColumnsOpt([]string{"email"}), // no need take email column
	)

	query, args, err := squirrel.Select(st.GetColumns()...).
		From("users").
		Where(squirrel.Eq{"id", 1}).
		Limit(1).
		ToSql()
	if err != nil {
		return nil, err
	}

	err = st.SelectOne(&user, query, args...)
	if err != nil {
		return nil, err
	}

	return &user, nil
}
```

## List Opt
- `SerialColumnOpt`: define serial/auto increment column of table, which is ignore when execute insert command. Default: `id`.
- `NullableColumnsOpt`: define list of column when execute insert/update command is null/nil-able, separate by comma.
- `DateTimeColumnsOpt`: define list of column that need convert from/to `time.Time` when execute command or scan response value.
- `AutoCreateDateTimeColumnsOpt`: define list of column is auto-fill current time if value nil when execute insert command.
- `AutoUpdateDateTimeColumnsOpt`: define list of column is auto-fill current time if value nil when execute insert/update command.
- `AllowColumnsOpt`: define list of column that need scan type and value. This will help you control `Prepare` method better without duplicate define struct. This opt has higher priority than `IgnoreColumnsOpt`.
- `IgnoreColumnsOpt`: define list of column that no need scan type and value. This will help you control `Prepare` method better without duplicate define struct.

---
[Back to Home](https://github.com/wizk3y/go-sqltool)