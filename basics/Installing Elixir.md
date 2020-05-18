---
tags: [Notebooks/Technology/Elixir]
title: Installing Elixir
created: '2020-05-02T12:12:01.876Z'
modified: '2020-05-17T06:35:04.539Z'
---

# Installing Elixir

Use ASDF to manage multiple versions of Erlang and Elixir
- https://github.com/asdf-vm/asdf

Adding plugins
```
asdf plugin-add erlang https://github.com/asdf-vm/asdf-erlang.git
asdf plugin-add elixir https://github.com/asdf-vm/asdf-elixir.git
```

Install additional dependencies
https://github.com/asdf-vm/asdf-erlang

Install Erlang and Elixir
```
asdf install erlang 22.3.2 
asdf install elixir 1.10.3
```

Set global version of the languages
```
asdf global erlang 22.3.2
asdf global elixir 1.10.3
```

Install Mix and Phoenix Commands
```
mix local.hex
mix archive.install hex phx_new 1.5.1
```

