---
title: Pattern match with the end of a pipe
date: 2021-01-01
description: "It's handy right?"
tags:
  - Elixir
---

# Pattern match in a pipe by using cond

https://elixirforum.com/t/match-on-the-end-of-a-pipe-matchpipe/20650
https://stackoverflow.com/questions/36807608/pattern-matching-during-pipe

You need to pipe into Integer.parse first and then into case:

```elixir
defmodule MyInteger do
  def parse(string) do
    string
    |> Integer.parse
    |> case do
         {int, _} -> int
         _ -> nil
       end
  end
end
```
