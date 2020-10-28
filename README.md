# Tetris Phoenix LiveView

Tetris In LiveView videos can be found at [Groxio Learning YouTube](https://www.youtube.com/c/Groxio) - Two videos per week will be released starting __10/26/2020__ through __12/14/2020__, and you can follow theses videos on http://bit.ly/GroxioTetris.

The Tetris in Elixir is an updated live coding video series by [Bruce Tate](https://twitter.com/redrapids) using Phoenix LiveView version 1.5. This series supports the [Groxio Learning](https://grox.io/) mentoring programs at Red Bank High School, Chattanooga's Gig City Elixir, and ElixirChatt.

Join our Discord Community! https://discord.gg/VQnJQAu

Skills include Reducers and protocols. LiveView links, events, lifecycle, and code organization.

Along the way, we’ll learn a little Elixir, some Phoenix, and also LiveView.

## 1 Tetris LiveView - Introduction and Reducers

### Prerequisite

  * Elixir: https://elixir-lang.org/install.html
  * Postgres: https://www.postgresql.org/download/
  * Phoenix: https://hexdocs.pm/phoenix/installation.html
  * Elixir - Getting Started: https://elixir-lang.org/getting-started/introduction.html
  * Elixir School: https://elixirschool.com/en/
  * Phoenix - Up and Running: https://hexdocs.pm/phoenix/up_and_running.html#content
  * Ecto - Getting Started: https://hexdocs.pm/ecto/getting-started.html

It create a new phoenix project include LiveView. After we guarantee our environment has Elixir, Postgres and Phoenix are all installed, we can continue and run this command:
```shell
 $ mix phx.new

                                  mix phx.new                                   

Creates a new Phoenix project.

It expects the path of the project as an argument.

    mix phx.new PATH [--module MODULE] [--app APP]

A project at the given PATH will be created. The application name and module
name will be retrieved from the path, unless --module or --app is given.

## Options

  • --live - include Phoenix.LiveView to make it easier than ever to build
    interactive, real-time applications
```

The Ecto package by default use a Postgres Database, so you need to install the database and guarantee your service it's running, after that you maybe change your credentials to access your database on `config/dev.exs` file. Now with a correct configuration to access our database, it could be run the `ecto.create` to create the database, but before we continue and create our database, you need to be more familiar with ecto task are available to us, so run:

```shell
mix ecto
Ecto v3.5.3
A toolkit for data mapping and language integrated query for Elixir.

Available tasks:

mix ecto.create        # Creates the repository storage
mix ecto.drop          # Drops the repository storage
mix ecto.dump          # Dumps the repository database structure
mix ecto.gen.migration # Generates a new migration for the repo
mix ecto.gen.repo      # Generates a new repository
mix ecto.load          # Loads previously dumped database structure
mix ecto.migrate       # Runs the repository migrations
mix ecto.migrations    # Displays the repository migration status
mix ecto.reset         # Alias defined in mix.exs
mix ecto.rollback      # Rolls back the repository migrations
mix ecto.setup         # Alias defined in mix.exs
```

Now running the `ecto.create`:
```shell
$ mix ecto.create
The database for Tetris.Repo has been created
```

Before jump and write some code, its very important you need to be more familiar with `IEx` is an abbreviation of "Interactive Elixir", and the `IEx Commands` and most `Data Types`.

  * Tetris Guideline: https://tetris.fandom.com/wiki/Tetris_Guideline
  * Funcional Programming: https://inquisitivedeveloper.com/lwm-elixir-3/
  * IEx: https://inquisitivedeveloper.com/lwm-elixir-5/
  * IEx and Code Comments: https://inquisitivedeveloper.com/lwm-elixir-12/
  * Data Types: https://inquisitivedeveloper.com/lwm-elixir-6/
  * Modules and Functions - Part 1: https://inquisitivedeveloper.com/lwm-elixir-10/
  * Modules and Functions - Part 2: https://inquisitivedeveloper.com/lwm-elixir-11/
  * Tuples: https://inquisitivedeveloper.com/lwm-elixir-13/
  * Introducing reducees: http://blog.plataformatec.com.br/2015/05/introducing-reducees/
  * The Tuple Module: https://inquisitivedeveloper.com/lwm-elixir-26/
  * The Enum Module Part 1: https://inquisitivedeveloper.com/lwm-elixir-28/
  * The Enum Module Part 2: https://inquisitivedeveloper.com/lwm-elixir-31/
  * The Enum Module Part 3: https://inquisitivedeveloper.com/lwm-elixir-33/
  * The Enum Module Part 4: https://inquisitivedeveloper.com/lwm-elixir-36/

We will initiate our code creating the first module, called Point.
```shell
$ touch lib/tetris/point.ex
```
The module is a bucket for our code where we will put everything on when the code make sense and belonging of the rest of code.

For a build design of our code and module we almost have three kind of functions.

  * Constructor - It will be our initialize function.
  * Reducer - The reducer will receive as a parameter a kind of data type and will return the same kind of data type, but our reducer function will change doing transform our data and return a new data.

#### IEX Session
```elixir
$ iex -S mix
Erlang/OTP 23 [erts-11.1.1] [source] [64-bit] [smp:12:12] [ds:12:12:10] [async-threads:1] [hipe]

Interactive Elixir (1.11.1) - press Ctrl+C to exit (type h() ENTER for help)
iex> Tetris.Point.origin()
{0, 0}
iex> recompile
Compiling 1 file (.ex)
:ok
iex> alias Tetris.Point
Tetris.Point
iex> Point.origin()
{0, 0}
iex> point = Point.origin()
{0, 0}
iex> Point.left(point)
{-1, 0}
iex> recompile
Compiling 1 file (.ex)
:ok
iex> Point.origin() |> Point.right()
{1, 0}
iex> point |> Point.right() |> Point.right() |> Point.down()
{2, 1}
iex> point
{0, 0}
iex> add = fn x, y -> x + y end
#Function<43.97283095/2 in :erl_eval.expr/5>
iex> list = [1,2,3]
[1, 2, 3]
iex> Enum.reduce(list, 0, add)
6
iex> 0 |> add.(1) |> add.(2) |> add.(3)
6
```
