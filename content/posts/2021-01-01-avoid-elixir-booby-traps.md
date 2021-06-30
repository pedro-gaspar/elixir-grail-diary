---
title: Avoid elixir booby traps
date: 2021-01-01
description: 'Coming from an object oriented background, you will fall into elixir booby traps.'
tags:
  - Elixir
---

# Assignment is more like 


# Quotes and double quotes aren't strings

# Change variables inside ifs

if (x ==1) do
  x = 2
  y = 3
else
  x = 4
  y = 5
end

# Set a value in a map

map['x'] = y (this is not an array and not and object), you have other ways like

Map.put(map, x, y)
map = %{map | x: y}

# for is not the for you know

for i <- 1..3 do
  map = %{map | x: i}
end

Doesn't change anything

For that you need to use Enum.reduce or recursion

# Need state?

You can pass temporary state as a function

or store it in an agent

or store it in a gen server
