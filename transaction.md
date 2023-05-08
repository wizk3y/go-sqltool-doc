# Transaction
[Back to Home](https://github.com/wizk3y/go-sqltool)

---
In special curcumstance, developer might need using transaction. `SQLTool` has `Begin`, `Commit`, `Rollback` to support developer execute transaction. `Rollback` method should always run in `defer`, which make sure that any error will close transaction to prevent database deadlock.

```go
type User struct {
	ID    int64  `json:"id"`
	Name  string `json:"name"`
	Email string `json:"email"`
}

func BatchInsert(ctx context.Context, db *sql.DB, users []*User) error {
	st := sqltool.NewTool(ctx, db)
	err := st.Begin()
	if err != nil {
		return err
	}

	defer func() {
		if err != nil {
			st.Rollback()
		}
	}()

	st.PrepareInsert(&User{})

	qb := squirrel.Insert("users").
		Columns(st.GetColumns()...)

	for _, u := range users {
		qb = qb.Values(st.PrepareValues(u)...)
	}

	query, args, err := qb.ToSql()
	if err != nil {
		return err
	}

	_, err = st.Exec(query, args...)
	if err != nil {
		return err
	}

	err = st.Commit()
	if err != nil {
		return err
	}

	return nil
}
```

---
[Back to Home](https://github.com/wizk3y/go-sqltool)