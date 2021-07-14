---
title: Debugging Elixir with Visual Studio Code
date: 2021-07-11
description: 'Diferent options to debug Elixir'
tags:
  - Elixir
  - VSCode
  - Debugging
---

> Debugging is being the detective in a crime movies where you are also the murderer.

Detectives have tools to help them. They can be old fashioned Sherlock Holmes and need a magnifying glass or go full CSI and have a computer that cross checks fingerprints showing one at a time in the display 🤦

You will for sure commit crimes during development so what tools does Elixir have to get your back?

# Old school

Well even though there are a lot of better tools for that, sometimes the good old print will save your day.

```elixir
username = "º-º"
IO.puts("------>" <> username)
```

The problem with it is that we have to print a sting or string-compatible format.

# Old school with steroids

Doing `IO.puts` is not ideal, specially if you want to print info about some more complex data structure like a map or a tuple.

IO.inspect got your back. It will do the proper conversion to a string and display useful information.

```elixir
conn = %{status: 504}
IO.inspect(conn)
```

But what about happens during a pipeline?

```elixir
["some", "weird", "debugging"]
|> does_some_stuff()
|> does_even_some_stuff()
|> does_even_more_stuff()
```

Imutability is awesome, it allows you to know that an error is happening in one of the above functions. But which one?

You could do the following:

```elixir
stuff = 
  ["some", "weird", "debugging"]
  |> does_some_stuff()

IO.inspect(stuff)

  stuff
  |> does_even_some_stuff()
  |> does_even_more_stuff()
```

But `IO.inspect` is a perfect match for this because it returns it's argument unchanged so you can just do:

```elixir
["some", "weird", "debugging"]
|> does_some_stuff()
|> IO.inspect()
|> does_even_some_stuff()
|> does_even_more_stuff()
```

Awesome right?

You can even add labels to help you during this kind of debugging:

```elixir
["some", "weird", "debugging"]
|> does_some_stuff()
|> IO.inspect(label: "suspect num 1")
|> does_even_some_stuff()
|> IO.inspect(label: "suspect num 2")
|> does_even_more_stuff()
```

# IEX.pry

Sometimes instead of printing every single variable, you want to be able to check some variables or see the results of applying a function given a specific state.

`IEX.pry` will help you with that.

You are able to stop the application in a point in time.

```elixir
defmodule Crime do
  def double_or_nothing(number) do
    number / 0 + 10 
  end
end
```

To use `IEX.pry` you just need to add the following to the place you want to debug

```elixir
require IEX; IEX.pry
```

So in this case is just doing:

```elixir
defmodule Crime do
  def double_or_nothing(number) do
    require IEx; IEx.pry
    number / 0 + 10 
  end
end
```

Then you can start a mix session:

```elixir
$ iex -S mix

iex(1)> Crime.double_or_nothing(10)
Break reached: Crime.double_or_nothing/1 (iex:2)
pry(1)> i number
Term
  10
Data type
  Integer
Reference modules
  Integer
Implemented protocols
  IEx.Info, Inspect, List.Chars, String.Chars
pry(2)> number
10
pry(3)> number + 10
20
pry(4)> h # if you need help
...
pry(5)> respawn()
```

You finish by calling `respawn`. 

This is great but has some cons:

- You have to change the code you want to debug
- You can only inspect places where you have a pry, you can't step or add breakpoints like other debuggers you might have used

If you want to have multiple debug steps you can add multiple `require IEx; IEx.pry` and continue to the next after one finishes.

# IEX break!

Instead needing to hardcode the IEX.pry breakpoints you can just set a breakpoint before executing the code.

Using the same example:

```elixir
defmodule Crime do
  def double_or_nothing(number) do
    require IEx; IEx.pry
    number / 0 + 10 
  end
end
```

You can then:

```elixir
$ iex -S mix

iex(1)> break! Crime.double_or_nothing/1
1
iex(2)> Crime.double_or_nothing(20)
Break reached: Crime.double_or_nothing/1 (lib/crime.ex:2)

    1: defmodule Crime do
    2:   def double_or_nothing(number) do
    3:     number / 0 + 10
    4:   end
pry(1)> i number
Term
  20
Data type
  Integer
Reference modules
  Integer
Implemented protocols
  IEx.Info, Inspect, List.Chars, String.Chars
pry(2)> whereami
Location: lib/crime.ex:2

    1: defmodule Crime do
    2:   def double_or_nothing(number) do
    3:     number / 0 + 10
    4:   end

    (crime 0.1.0) Crime.double_or_nothing/1
pry(3)> respawn()
```

You also specify in break the number of times it will break.

# Using IEx.pry in tests

If you want to debug a test if you add `require IEx; IEx.pry` to it, when you run the tests with `mix test` you will get the following error:

```elixir
Cannot pry #PID<0.123.0> at Crime.CrimeTest ... 
Is an IEx shell running?
```

This is because you are not inside an interactive shell session.

Simple enough, just run

```elixir
iex -S mix test
```

# Avoid timeouts when running tests

To avoid timeout run tests with the trace option

```elixir
$ iex -S mix test --trace
```

# VSCode and ElixirLS

Left the best for last. If you are a Visual Studio Code user you are in a good place for debugging Elixir.

First step is to install the ElixirLS extension.

!()[images/elixirls.png]

Then you need to add a configuration file to setup debug. Click on "Run and Debug"

!()[images/vscode-debug1.png]

After that click on "create a lauch.json file"

!()[images/vscode-debug2.png]

It will create a default for Elixir.

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "type": "mix_task",
      "name": "mix (Default task)",
      "request": "launch",
      "projectDir": "${workspaceRoot}"
    },
    {
      "type": "mix_task",
      "name": "mix test",
      "request": "launch",
      "task": "test",
      "taskArgs": [
        "--trace"
      ],
      "startApps": true,
      "projectDir": "${workspaceRoot}",
      "requireFiles": [
        "test/**/test_helper.exs",
        "test/**/*_test.exs"
      ]
    },  
  ]
}
```

I like to add a debug mix task where I can run some code I want to debug. For that, add the following to the lauch.json file.

```json
    {
      "type": "mix_task",
      "name": "debug",
      "request": "launch",
      "startApps": true,
      "projectDir": "${workspaceRoot}",
      "task": "debug",
    },
```

And inside your project add:

**lib/mix/tasks.debug**

```elixir
defmodule Mix.Tasks.Debug do
  use Mix.Task

  @impl Mix.Task
  def run(_args) do
    IO.puts("debugging...")

    # Code to test
    Crime.double_or_nothing(10)
  end
end
```

With the configuration you can now debug when running a test or by invoking the code you want to debug in the debug mix task.

Add a breakpoint(s) to the code you want to check.

!()[images/vscode-debug3.png]

And then click to debug:

!()[images/vscode-debug4.png]

It will stop in the breakpoint where you can inspect values, add watches and step into and over the next lines.

!()[images/vscode-debug5.png]

Now with these tools you are ready to solve any crime you commit in your code.


**References**

- https://elixir-lang.org/getting-started/debugging.html
- http://blog.plataformatec.com.br/2016/04/debugging-techniques-in-elixir-lang/
- https://adamdelong.com/iex-pry-test/
