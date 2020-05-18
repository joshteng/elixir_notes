---
tags: [Notebooks/Technology/Elixir/Ecto]
title: Ecto Migrations
created: '2020-05-17T07:52:24.987Z'
modified: '2020-05-17T07:54:11.661Z'
---

# Ecto Migrations

## Not null
```
defmodule Rumbl.Repo.Migrations.CreateCategories do
  use Ecto.Migration

  def change do
    create table(:categories) do
      add :name, :string, null: false # not null

      timestamps()
    end

  end
end

```

## Relationship
```
defmodule Rumbl.Repo.Migrations.CreateVideos do
  use Ecto.Migration

  def change do
    create table(:videos) do
      add :url, :string
      add :title, :string
      add :description, :text
      add :user_id, references(:users, on_delete: :nothing)

      timestamps()
    end

    create index(:videos, [:user_id])
  end
end

```
