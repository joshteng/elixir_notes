---
tags: [Notebooks/Technology/Elixir, Notebooks/Technology/Elixir/Phoenix]
title: Phoenix Basic Commands
created: '2020-05-06T08:44:38.237Z'
modified: '2020-05-17T07:31:52.353Z'
---

# Phoenix Basic Commands

## Create new app
```
mix phx.new simple_greet
```

## Create DB
```
mix ecto.create
```

## DB Migrations
```
mix ecto.migrate
mix ecto.rollback
```

## Installing Dependencies
```
mix deps.get
```

## Other Aliases
```
mix setup # installs dependencies, create DB, migrate DB, seed data and compile assets
mix ecto.setup # create DB, migrate DB, seed data
mix ecto.reset # drop DB, create DB, migrate DB, seed data
mix test # create test DB, migrate test DB, run tests
```

## Start Server
```
mix phx.server
iex -S mix phx.server # this combines a REPL with the server
```

## Hot Reloading
- Hot reloading on works for views and templates
- Everything else requires a restart or recompile
- If started server using `iex -S mix phx.server`, simply run:
```
recompile()
```



