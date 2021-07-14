---
title: The Elixir Tutorial
date: 2021-07-03
description: "Learn by teaching."
images:
- images/featured/twitter.jpeg
tags:
  - Elixir
  - The Elixir Tutorial
---

# for post <- 1..4, do: IO.puts "The Elixir Tutorial." 

Richard Feynman is a known physicist that promoted the following learning technique, known as **The Feynman Technique**.

- Pretend to teach a concept you want to learn about to a student in the sixth grade.
- Identify gaps in your explanation. Then, go back to the source material to understand it better.
- Organize and simplify.
- Transmit 

![](https://media.giphy.com/media/l2R06HpuWmc3pnBks/giphy.gif)

So I'm learning Elixir and putting my knowledge to the test by teaching at the same time. 

> Learning in the open.

I'm going to start a series of posts around building an application with Elixir. For that, we will try to build a blog brother app, **the grail-toolbox**.

It will be an app that will have the following features:

- post into my Twitter account a tweet every time there's a new blog post
- post in medium a copy of each new blog post
- create a JSON API with all recent tweets from José Valim
- scrape posts from Dashbit, store a summarized version in a database to expose has a Graphql endpoint

Let's learn and have fun. 😎

Subject to changes along the way. 

![](https://media.giphy.com/media/03L3XIy2uKaLE5TIfG/giphy.gif)

# "The Elixir Tutorial" | > Enum.map(&IO.puts/1)

1..4 -> [The Elixir Tutorial](/posts/2021-07-03-the-elixir-tutorial/)

2..4 -> [Installing Elixir in your local machine](/posts/2021-07-04-elixir-tutorial-install-elixir/)

3..4 -> [You don't need to install Elixir](/posts/2021-07-05-elixir-tutorial-elixir-in-a-box/)

4..4 -> [Running Elixir](/posts/2021-07-06-elixir-tutorial-running-elixir/)
