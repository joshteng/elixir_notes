---
tags: [Notebooks/Technology/Elixir]
title: Pig Latin
created: '2020-05-05T16:47:47.126Z'
modified: '2020-05-05T16:49:28.905Z'
---

# Pig Latin

```elixir
defmodule PigLatin do
  defp is_vowel?(first_char) do
    vowels = ["a", "e", "i", "o", "u"]
    Enum.member?(vowels, String.downcase(first_char))
  end

  defp format(list) do
    IO.inspect(List.to_string(list))
  end

  defp process([first_char | remainder], new_string_list) do
    cond do
      is_vowel?(first_char) and length(new_string_list) > 0 -> format([first_char] ++ remainder ++ new_string_list ++ ["a", "y"])
      is_vowel?(first_char) -> format([first_char] ++ remainder ++ new_string_list)
      true -> process(remainder, new_string_list ++ [first_char])
    end
  end

  defp process(_, new_string_list), do: format(new_string_list ++ ["a", "y"])

  def convert(word) when is_binary(word) do
    word
    |> String.graphemes()
    |> process([])
  end

  def convert(_) do
    IO.inspect("Please pass in a string string not a wannabe string")
  end
end
```
