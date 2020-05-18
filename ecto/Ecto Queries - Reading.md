---
tags: [Notebooks/Technology/Elixir, Notebooks/Technology/Elixir/Ecto]
title: Ecto Queries - Reading
created: '2020-05-17T05:41:34.280Z'
modified: '2020-05-17T14:00:13.727Z'
---

# Ecto Queries - Reading

## Retrieving Single Row
```
alias Rumbl.Repo

video = Repo.get(Rumbl.Multimedia.Video, video.id)
```

alternatively, create convenience function `get_video/1` in the relevant context
```
defmodule Rumbl.Multimedia do
  alias Rumbl.Repo
  alias Rumbl.Multimedia.Video

  def get_video!(id) do
    Repo.get(Video, id)
  end
end

video = Rumbl.Multimedia.get_video!(video.id)
```

## Retrieving Associated Data
```
video = Rumbl.Repo.get(Rumbl.Multimedia.Video, id)
video = Rumbl.Repo.preload(video, :user)
video.user

# or in one step
video = Repo.one(from v ​in​ Video, ​limit:​ 1, ​preload:​ [​:user​])

# of manually specifying join
Repo.all from v ​in​ Video,
​          ​join:​ u ​in​ assoc(v, ​:user​),
​          ​join:​ c ​in​ assoc(v, ​:category​),
​          ​where:​ c.name == ​"​​Comedy"​,
          ​select:​ {u, v} # returns a list of tuple {user, video} structs, we can also return as list of map
```

## Retrieving Associated Data without storing it in the video struct
```
alias Rumbl.Repo

query = Ecto.assoc(video, :user)
user = Repo.one(query)
```

## Retrieving Back Reference structs
```
  def list_user_videos(%User{} = user) do
    Video
    |> user_video_query(user)
    |> Repo.all()
  end

  def get_user_video!(%User{} = user, id) do
    Video
    |> user_videos_query(user)
    |> Repo.get!(id)
  end

  defp user_videos_query(query, %User{id: user_id}) do
    from(v in query, where: v.user_id == ^user_id)
  end
```

## Using Ecto Queries


```
import Ecto.Queries
alias Rumbl.Repo
alias Rumbl.Multimedia.Category

query = from c in Category, select: c.name

Repo.all(query)

Repo.all(from c in Category,
          order_by: c.name,
          select: {c.name, c.id})

# queries are composable
query = Category
query = from c ​in​ query, ​order_by:​ c.name
query = from c ​in​ query, ​select:​ {c.name, c.id}
Repo.all(query)
```
- from receives a `queryable`, an elixir protocol like `Enum`
```
Repo.all(Category)
Repo.all(query)
```
- Both `Category` and `query` are `queryable` protocols


```
username = "joshteng"
Repo.one(from u ​in​ User, ​where:​ u.username == ^username)
```
- if select is omitted, a schema struct will be returned
- Repo one doesnt mean return the first but return one. If there are more it would fail
- username is a value but can also be an expression

```
video = Repo.one(from v ​in​ Video, ​limit:​ 1)
```
- this won't throw an error if there are more than one

```
users_count = from u ​in​ User, ​select:​ count(u.id)
j_users = from u ​in​ users_count, ​where:​ ilike(u.username, ^​"​​%j%"​)
```
- composing using keyword syntax

```
User
|> select([u], count(u.id)) \
|> where([u], ilike(u.username, ^​"​​j%"​) ​or​ ilike(u.username, ^​"​​c%"​))
|> Repo.one()
```
- composing using pipe


```
from u ​in​ User,
​ 	     ​where:​ fragment(​"​​lower(username) = ?"​, ^String.downcase(name))
```
- SQL fragments
- Using a fragment allows us to construct a fragment of SQL for the query but safely interpolate the String.downcase(name) code using a prepared statement. Whether the interpolated values are Ecto query expressions or SQL fragments, Ecto safely escapes all interpolated values.

```
Ecto.Adapters.SQL.query(Repo, ​"​​SELECT power($1, $2)"​, [2, 10])
```
- Running SQL directly



