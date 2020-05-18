---
tags: [Notebooks/Technology/Elixir, Notebooks/Technology/Elixir/Phoenix]
title: Phoenix Cookbook - Snippets and More
created: '2020-05-17T07:26:55.346Z'
modified: '2020-05-17T07:47:56.498Z'
---

# Phoenix Cookbook - Snippets and More

## Session
```
# conn is the first argument to all these functions. examples shown are used with Elixir's pipe operator
put_session(:user_id, user.id)
configure_session(renew: true)
configure_session(conn, drop: true)
delete_session(conn, :user_id)
```

## Flash
```
# conn is the first argument to all these functions. examples shown are used with Elixir's pipe operator
put_flash(:error, "You must be logged in to access that page")
```
