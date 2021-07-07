---
title: Enable history in iex
date: 2021-06-28
description: 'This is handy.'
tags:
  - Elixir
---

After spending some time in `iex`, you want to run a past command, especially from previous sessions.

Being used to `irb` or `ipython`, you hit the up arrow, and nothing happens...

![](https://media.giphy.com/media/oziNormWuA6JrnbzY8/giphy.gif)

Well, that is not enabled by default.

For that to work, add the following to your `.zshrc` or `.bashrc`

```sh
export ERL_AFLAGS="-kernel shell_history enabled"
```

Stackoverflow [link](https://stackoverflow.com/questions/45405070/how-do-i-save-iex-history) for reference.
