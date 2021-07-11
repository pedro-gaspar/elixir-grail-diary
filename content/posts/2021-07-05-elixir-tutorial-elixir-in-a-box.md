---
title: Elixir in a box
date: 2021-07-05
description: "You don't need to install Elixir"
tags:
  - Elixir
  - The Elixir Tutorial
---

# for post <- 3..4, do: IO.puts "The Elixir Tutorial" 

![](https://media.giphy.com/media/6AFldi5xJQYIo/giphy.gif)

If you have `docker` and `docker-compose` you don't necessarily need to install Elixir from scratch. There's an easy way, you can let containers do the heavy lifting.

Just create a new app, for instance:

```sh
$ mix new elixir_docker
```

Then create a docker file based on the Elixir image:

**Dockerfile**

```sh
FROM bitwalker/alpine-elixir:latest

WORKDIR /app

COPY mix.exs .

CMD mix deps.get
```

Then a docker-compose file. This will be useful later on when you add more pieces to your system like a database.


**docker-compose.yml**

```sh
version: '3.6'
services:
  app:
    build: .
    environment:
      MIX_ENV: dev
    volumes:
      - .:/app
```

And then you just need to build your container and run it.

```sh
$ docker-compose build
$ docker-compose run app iex -S mix
```

And just like that, you have a running `iex`.

![](https://media.giphy.com/media/3oriNYQX2lC6dfW2Ji/giphy.gif)

Well, depending on your network to download the images and if you already have `docker` and `docker-compose` installed. But you get my point 😏

And you are ready to go. For now I'll keep using Elixir locally on my Ubuntu, you can call me old school, but this is surely a great alternative.


For reference: [Development environment for Elixir + Phoenix with Docker and Docker-compose](https://dev.to/hlappa/development-environment-for-elixir-phoenix-with-docker-and-docker-compose-2g17)


# "The Elixir Tutorial" | > Enum.map(&IO.puts/1)

1..4 -> [The Elixir Tutorial](/posts/2021-07-03-the-elixir-tutorial/)

2..4 -> [Installing Elixir in your local machine](/posts/2021-07-04-elixir-tutorial-install-elixir/)

3..4 -> [You don't need to install Elixir](/posts/2021-07-05-elixir-tutorial-elixir-in-a-box/)

4..4 -> [Running Elixir](/posts/2021-07-06-elixir-tutorial-running-elixir/)
