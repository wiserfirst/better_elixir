# [fit] Practical tips about with

by Qing Wu

^
Hello everyone, my name ...
I work for ...

---

# Why

```elixir
{:ok, result} | {:error, reason}
```

---

# Examples

```elixir
HTTPoison.post(url, body, headers)

Poison.decode(response)

Repo.create(User, attributes)
```

---

# conditionals

```elixir
case function1() do
  {:ok, result} ->
    ...
    {:ok, "all good"}

  {:error, reason} ->
    ...
    {:error, reason}
end
```

---

# nested conditionals

```elixir
case function1() do
  {:ok, result1} ->
    case function2() do
      {:ok, result2} ->
        ...
        {:ok, "all good"}

      {:error, reason2} ->
        ...
        {:error, reason2}
    end

  {:error, reason1} ->
    ...
    {:error, reason1}
end
```

---

# use with
☺️

```elixir
with {:ok, result} <- function1() do
  ...
end

# or

with {:ok, result} <- function1() do
  ...
else
  {:error, reason} ->
    ...
    {:error, reason}
end
```

---

😍

```elixir
with {:ok, result1} <- function1(),
     {:ok, result2} <- function2() do
  ...
else
  {:error, reason} ->
    ...
    {:error, reason}
end
```

---

# Watch out

💥

```elixir
{:ok, result} = function1()
...
```

---

# `else` in `with`
🤔

```elixir
with {:ok, result} <- function1() do
  ...
else
  {:error, reason} -> {:error, reason}
end
```

---

# Be careful

❗️

```elixir
with {:ok, result} <- function1() do
  ...
else
  {:error, :specific_error} ->
    ...
end
```
---

# Have a match all clause

🙌

```elixir
with {:ok, result} <- function1() do
  ...
else
  {:error, :specific_error} ->
    ...
    {:error, "function1 failed because ..."}

  {:error, reason} ->
    {:error, reason}
end
```

---

# Do NOT abuse `with`

🤦

```elixir
with result <- function1() do
  ...
end
```

---

# does not cover

1. Just normal pattern matching and tagged tuples are just a convention, not enforced.
2. Doesn’t handle exceptions, functions have to return a value

---

# For more details

👍

```bash
$ iex
iex(1)> h with
```

---

# continue piping

```elixir
user
|> Repo.update(attributes)
|> fun1()

def fun1({:ok, term}), do: my_action(term) # do something with term
def fun1({:error, reason}), do: {:error, reason}
```


---

# Result.map
⭐️

```elixir

defmodule Vamp.Data.Result do
  def map({:ok, value}, func) when is_function(func, 1), do: value |> func.()
  def map({:error, reason}, _func), do: {:error, reason}
end

user
|> Repo.update(attributes)
|> Result.map(&my_action/1)

```

---

# eliminate conditionals

```elixir
if condition do
  true_fun()
else
  false_fun()
end

def process(true), do: true_fun()
def process(false), do: false_fun()
```

