---
tags: [Notebooks/Technology/Elixir/Ecto]
title: Ecto Validation and Constraints
created: '2020-05-17T13:57:41.389Z'
modified: '2020-05-17T17:12:39.968Z'
---

# Ecto Validation and Constraints

Application Layer Constraints
```
validate_required([​:name​, ​:username​])
validate_length(​:username​, ​min:​ 1, ​max:​ 20)
```
- validates even before attempting Repo.update/1 or Repo.insert/1

Database Constraints
```
unique_constraint(​:username​)
assoc_constraint(​:category​)
```
- validates only after attempting to save. basically this cathes Ecto Errors and converts them into friendly validation error messages
- so the app won't crash but we get {:error, _} back

## Validating Uniqueness on One Column
- an index should be present and added during migration
- unique_constraint can also be added to schema if it's a user input to prevent crashing

```
defmodule Rumbl.Repo.Migrations.CreateUsers do
  use Ecto.Migration

  def change do
    create table(:users) do
      add :name, :string
      add :username, :string, null: false
      add :password_hash, :string

      timestamps()
    end

    create unique_index(:users, [:username])
  end
end

def changeset(user, attrs) do
  user
  |> cast(attrs, [:name, :username])
  |> validate_required([:name, :username])
  |> unique_constraint(:username)
  |> validate_length(:username, min: 1, max: 20)
end
```


## Foreign Key Uniqueness

- Create a foreign key reference in migration
- Raises an error if violates uniqueness
- To not raise error and allow users to modify, perform unique constraint in schema. If it's not user fixable, let it crash and get notified

```
defmodule Rumbl.Repo.Migrations.AddCategoryIdToVideo do
  use Ecto.Migration

  def change do
    alter table(:videos) do
      add :category_id, references(:categories)
    end
  end
end


defmodule Rumbl.Multimedia.Video do
  use Ecto.Schema
  import Ecto.Changeset

  schema "videos" do
    field :description, :string
    field :title, :string
    field :url, :string
    field :slug, :string
    # field :user_id, :id
    belongs_to :user, Rumbl.Accounts.User
    belongs_to :category, Rumbl.Multimedia.Category

    timestamps()
  end

  def changeset(video, attrs) do
    video
    |> cast(attrs, [:url, :title, :description, :category_id])
    |> validate_required([:url, :title, :description])
    |> assoc_constraint(:category)
    |> slugify_title()
  end
end
```

Foreign Key uniqueness constraints also apply when deleting; 3 strategies to handling:
1. use changeset constraints to handle aka foreign_key_constraint
1. configure the database to either cascade the deletions or simple make the the foreign key field NULL on delete
1. set up :on_delete in the schema (has_many or belongs_to). This is delegates the handling to the application layer which is not favorable unless the other 2 ways are not applicable.

First way
```
import Ecto.Changeset
alias Rumbl.Repo
alias Rumbl.Multimedia.Category

category = Repo.one(Category, limit: 1)
changeset = change(category)
changeset = foreign_key_constraint(changeset, :videos, name: :videos_category_id_fkey, message: "still exist")
Repo.delete(changeset)
```

Second Way
```
add ​:category_id​, references(​:categories​, on_delete: :nothing) # default behavior, raise error
add ​:category_id​, references(​:categories​, on_delete: :delete_all) # delete all the associated objects
add ​:category_id​, references(​:categories​, on_delete: :nilify_all) # change field to NULL
```


## Letting it Crash

Sometimes validations don't make sense, we want the application to actually crash.

The best way to differentiate when to handle errors to return friendly messages vs letting the application crash is to differentiate what is a user error and what is a application logic error. If it's application logic, we don't want to handle. We want to let it crash. If it's a user error, then we don't want to let it crash and return friendly error messages so that users can take appropriate actions to correct their mistakes
