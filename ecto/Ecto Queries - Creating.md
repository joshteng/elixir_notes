---
tags: [Notebooks/Technology/Elixir/Ecto]
title: Ecto Queries - Creating
created: '2020-05-17T09:32:04.453Z'
modified: '2020-05-17T17:20:12.422Z'
---

# Ecto Queries - Creating

## Raw Ecto
```
alias Rumbl.Accounts.User
alias Rumbl.Repo

Repo.insert(%User{name: "somename"})
```


## With Predefined Changesets
```
defmodule Rumbl.Accounts.User do
  alias Rumbl.Repo

  def changeset(user, attrs) do
    user
    |> cast(attrs, [:name, :username])
    |> do some validation and return changeset
    |> do some other stuff e.g. salting passwords, slugifying URL and return changeset
  end
end

defmodule Rumbl.Accounts do
  def create_user(attrs \\ %{}) do
    %User{}
    |> User.changeset(attrs)
    |> Repo.insert()
  end
end

Rumbl.Accounts.create_user(%{name: "Some name", username: "Some username"})
```

## Creating a One-to-Many association
First way
```
alias Ecto.Changeset
alias Rumbl.Repo

user = Rumbl.Accounts.get_user_by(username: "joshteng3")

changeset = video |> Changeset.change() |> Changeset.put_assoc(:user, user) |> Repo.update!()

# same as (uses the changeset we created in Video model)
changeset = video |> Rumbl.Multimedia.Video.changeset(%{}) |> Ecto.Changeset.put_assoc(:user, user) |> Repo.update!()

```



Second way
```
alias Ecto.Changeset
alias Rumbl.Repo

user = Rumbl.Accounts.get_user_by(username: "joshteng3")

video = video |> Changeset.change() |> Changeset.put_change(:user_id, user.id) |> Repo.update!()
```

## Validating Foreign Key Constraints
- make sure the association exists
- This is a changeset constraint that delegates the handling to the database, it means, it would let SQL throw an error, catching it and returning a user friendly changeset error message
- such constraint is defined in migration on the database level, which returns a constraint error, but to avoid letting the app crash, we also define a changeset constraint that would catch the error and return a changeset error message
```
  def changeset(video, attrs) do
    video
    |> cast(attrs, [:url, :title, :description, :category_id])
    |> validate_required([:url, :title, :description])
    |> assoc_constraint(:category)
  end
```
