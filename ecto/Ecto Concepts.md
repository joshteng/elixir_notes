---
tags: [Notebooks/Technology/Elixir/Ecto]
title: Ecto Concepts
created: '2020-05-17T07:37:46.151Z'
modified: '2020-05-17T14:17:00.843Z'
---

# Ecto Concepts

|Concept|Explanation|
|---|---|
|Repo|Is kinda like the executor to perform CRUD|
|Schema|Refers to the module that defines schemas and changesets|
|Structs|Schema structs are essentially structs but specific to the schema. They work like maps|
|Query|The module itself forms a query which is the foundation of more complicated queries|
|Changesets|These are things you work with when creating or updating structs. Forms also work with changesets. All validation errors are available in changesets|

Structs and Query are queryable protocols. Which means they can be used in an Ecto Query like Repo.all(queryable)

There are various ways to convert a struct into a changeset
1. cast(struct, attrs, [:attr_one, :attr_two])
2. change(struct)


