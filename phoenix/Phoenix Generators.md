---
tags: [Notebooks/Technology/Elixir, Notebooks/Technology/Elixir/Phoenix]
title: Phoenix Generators
created: '2020-05-17T06:35:09.987Z'
modified: '2020-05-17T07:57:27.973Z'
---

# Phoenix Generators

## Migration Generator
- Create migration only
```ex
mix ecto.gen.migration
​​mix​​ ​​ecto.gen.migration​​ ​​create_users​
​​mix​​ ​​ecto.gen.migration​​ ​​add_category_id_to_video​
```

## Full-on Generator
- Creates templates, view, controller, context, model (schema) and migration

HTML
```
mix phx.gen.html Context Model plural_model field:type

mix​​ ​​phx.gen.html​​ ​​Multimedia​​ ​​Video​​ ​​videos​​ ​​user_id:references:users​​ url:string​​ ​​title:string​​ ​​description:text
```

JSON
```
mix phx.gen.json Context Model plural_model field:type
```

> About Context
Sometimes it may be tricky to determine if two resources belong to the same context or not. The fact two resources are related in the database does not imply they belong in the same context. Otherwise, almost all schemas would be within the Accounts context, as the majority of entities in a system belong to a user.

> For example, users and videos are related, but they clearly belong in different contexts. On the other hand, categories and videos are also related, but we put them together, as categories are only available to multimedia resources and they do not bring much complexity on their own. In cases you are unsure how to group your resources, prefer distinct contexts per resource and refactor later if necessary. Otherwise you can easily end up with large contexts of loosely related entities. Similarly, if a context grows too large over time, you can always break it apart. To sum it up: When in doubt, put your new resource in its own context.

## Context Generator
- Create context, schema and migration
- If context already exists, inject the new category functions into the existing context
```
mix phx.gen.context Context Model model_plural field:type
```


## Schema Generator
- Create schema and migration
```
mix phx.gen.schema Context.Model model_plural field:type
```

