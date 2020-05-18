---
tags: [Notebooks/Technology/Elixir, Notebooks/Technology/Elixir/Ecto]
title: Creating a Mix Project that connects to DB
created: '2020-05-06T06:12:52.755Z'
modified: '2020-05-17T07:33:16.849Z'
---

# Creating a Mix Project that connects to DB

## Create new Mix Project
```sh
mix new <name>
```

## Including Dependencies
```elixir
# mix.exs
defp deps do
  [
    {:ecto_sql, "~> 3.2"},
    {:postgrex, "~> 0.15"}
    # {:dep_from_hexpm, "~> 0.3.0"},
    # {:dep_from_git, git: "https://github.com/elixir-lang/my_dep.git", tag: "0.1.0"}
  ]
end
```


## Installing Dependences
```sh
mix deps.get
```

## Create a Database Config File?
```sh
mix ecto.gen.repo -r Cinema.Repo
```

## Drop DB
```sh
mix ecto.drop
```

## Create Migration File
```sh
mix ecto.gen.migration create_mov
```

## Writing a Migration File to Create Table
```elixir
defmodule Cinema.Repo.Migrations.CreateMov do
  use Ecto.Migration

  def change do
    create table(:movies) do
      add :title, :string, null: false
      add :length_secs, :integer, null: false
    end
  end
end
```




## Run Migration
```elixir
mix ecto.migrate
```

## IEX a mix project so don't need to include things manually
```sh 
iex -S mix
```


## Initializing a Cinema.Movie Struct
```elixir
%Cinema.Movie{title: "The Avengers", length_secs: 18000000}
```

## Saving an Object
```elixir
%Cinema.Movie{}|> Ecto.Changeset.cast(%{title: "The Avengers", length_secs: 190000}, [:title, :length_secs]) |> Cinema.Repo.insert!
```

## Validating Objects
```elixir
%Cinema.Movie{} |>
  Ecto.Changeset.cast(%{length_secs: 190000}, [:title, :length_secs]) |>
  Ecto.Changeset.validate_required([:title])
```

## Creating Convenience Methods to deal with changesets
```elixir
  def changeset(struct, params) do
    struct
    |> cast(params, [:title, :length_secs])
  end
```


## Create
```elixir
%Cinema.Distributor{} |> cast(%{name: "Netflix"}) |> Cinema.Repo.insert!
```

### One to many
```elixir

Ecto.build_assoc(Cinema.Repo.get(Cinema.Distributor, 1), :movies, %{title: "Ultron", length_secs: 20000000}) |> Cinema.Repo.insert
```

### Many to many for simple join table
```elixir
Ecto.put_assoc(Cinema.Repo.get(Cinema.Distributor, 1), :movies, %{title: "Ultron", length_secs: 20000000}) |> Cinema.Repo.insert

movie |> Ecto.Changeset.cast_assoc(%{dist) # untested
```

## Read
```elixir
movie = Cinema.Repo.get(Cinema.Movie, 7)
```

### Preloading Associated Fields
```elixir
movie = movie |> Cinema.Repo.preload([:distributor])
```

### Filtering
```elixir
from(m in Cinema.Movie) |> where([m], m.length_secs > 10000000) |> Cinema.Repo.all

from(m in Cinema.Movie, where: m.length_secs > 10000000) |> Cinema.Repo.all 

from(m in Cinema.Movie, join: d in  Cinema.Distributor, on: m.distributor_id == d.id,  where: m.length_secs > 10000000, preload: [:distributor]) |> Cinema.Repo.all

from(m in Cinema.Movie, join: d in assoc(m, :distributor),  where: m.length_secs > 10000000, select: {m.title, d.name}) |> Cinema.Repo.all


```

## Update
```elixir
distributor |> Cinema.Distributor.changeset(%{name: "Crunchyroll"}) |> Cinema.Repo.update
```

