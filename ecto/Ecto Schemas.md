---
tags: [Notebooks/Technology/Elixir, Notebooks/Technology/Elixir/Ecto]
title: Ecto Schemas
created: '2020-05-17T05:53:42.964Z'
modified: '2020-05-17T17:14:07.309Z'
---

# Ecto Schemas

Schemas defines the struct and some convenience methods to be exposed as public API for other components in the app to use (most commonly context)

All database related business logic should be here

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


defmodule Rumbl.Multimedia.Video do
  use Ecto.Schema
  import Ecto.Changeset

  schema "videos" do
    field :description, :string
    field :title, :string
    field :url, :string
    # field :user_id, :id
    belongs_to :user, Rumbl.Accounts.User

    timestamps()
  end

  @doc false
  def changeset(video, attrs) do
    video
    |> cast(attrs, [:url, :title, :description])
    |> validate_required([:url, :title, :description])
  end
end
```

- use belongs_to on the model with the foreign key to associate a relationship with the associated object in this manner


