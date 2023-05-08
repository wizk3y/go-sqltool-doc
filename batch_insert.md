# Batch insert
[Back to Home](https://github.com/wizk3y/go-sqltool)

---
When insert more than one row to database, the simple `PrepareInsert` is not enough. At this time you need using `PrepareValues` inside loop after `PrepareInsert` to get value. Below is an example using with [squirrel](https://github.com/Masterminds/squirrel).

```go
type User struct {
	ID    int64  `json:"id"`
	Name  string `json:"name"`
	Email string `json:"email"`
}

func BatchInsert(ctx context.Context, db *sql.DB, users []*User) error {
	st := sqltool.NewTool(ctx, db)
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

    return nil
}
```

---
[Back to Home](https://github.com/wizk3y/go-sqltool)