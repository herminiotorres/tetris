# Tetris Phoenix LiveView

Tetris In LiveView videos can be found at [Groxio Learning YouTube](https://www.youtube.com/c/Groxio) - Two videos per week will be released starting __10/26/2020__ through __12/14/2020__, and you can follow theses videos on http://bit.ly/GroxioTetris.

The Tetris in Elixir is an updated live coding video series by [Bruce Tate](https://twitter.com/redrapids) using Phoenix LiveView version 1.5. This series supports the [Groxio Learning](https://grox.io/) mentoring programs at Red Bank High School, Chattanooga's Gig City Elixir, and ElixirChatt.

__Join our Discord Community!__ https://discord.gg/VQnJQAu

Skills include Reducers and protocols. LiveView links, events, lifecycle, and code organization.

Along the way, we’ll learn a little Elixir, some Phoenix, and also LiveView.

## Table of content
  * [01 - Introduction and Reducers](#01---introduction-and-reducers)
  * [02 - Structs](#02---structs)

## 01 - Introduction and Reducers

  * [__YouTube Link__](https://www.youtube.com/watch?v=8h8tWcz115M&list=PLKBMoE8mCkXi-sAkesjaUnDQqyrkAK8R5&index=1)

### Prerequisite

  * [Elixir](https://elixir-lang.org/install.html)
  * [Postgres](https://www.postgresql.org/download/)
  * [Phoenix](https://hexdocs.pm/phoenix/installation.html)
  * [Elixir - Getting Started](https://elixir-lang.org/getting-started/introduction.html)
  * [Elixir School](https://elixirschool.com/en/)
  * [Phoenix - Up and Running](https://hexdocs.pm/phoenix/up_and_running.html#content)
  * [Ecto - Getting Started](https://hexdocs.pm/ecto/getting-started.html)

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

#### Prior Knowledge

  * [Tetris Guideline](https://tetris.fandom.com/wiki/Tetris_Guideline)
  * [Funcional Programming](https://inquisitivedeveloper.com/lwm-elixir-3/)
  * [IEx](https://inquisitivedeveloper.com/lwm-elixir-5/)
  * [IEx and Code Comments](https://inquisitivedeveloper.com/lwm-elixir-12/)
  * [Data Types](https://inquisitivedeveloper.com/lwm-elixir-6/)
  * [Modules and Functions - Part 1](https://inquisitivedeveloper.com/lwm-elixir-10/)
  * [Modules and Functions - Part 2](https://inquisitivedeveloper.com/lwm-elixir-11/)
  * [Tuples](https://inquisitivedeveloper.com/lwm-elixir-13/)
  * [Introducing reducees](http://blog.plataformatec.com.br/2015/05/introducing-reducees/)
  * [The Tuple Module](https://inquisitivedeveloper.com/lwm-elixir-26/)
  * [The Enum Module Part 1](https://inquisitivedeveloper.com/lwm-elixir-28/)
  * [The Enum Module Part 2](https://inquisitivedeveloper.com/lwm-elixir-31/)
  * [The Enum Module Part 3](https://inquisitivedeveloper.com/lwm-elixir-33/)
  * [The Enum Module Part 4](https://inquisitivedeveloper.com/lwm-elixir-36/)

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

## 02 - Structs

#### Prior Knowledge

  * [Maps](https://inquisitivedeveloper.com/lwm-elixir-17/)
  * [The Map Module Part 1](https://inquisitivedeveloper.com/lwm-elixir-47/)
  * [The Map Module Part 2](https://inquisitivedeveloper.com/lwm-elixir-49/)
  * [Structs](https://inquisitivedeveloper.com/lwm-elixir-18/)
  * [Lists](https://inquisitivedeveloper.com/lwm-elixir-14/)
  * [Keyword Lists](https://inquisitivedeveloper.com/lwm-elixir-19/)
  * [Atom Part 1](https://elixir-lang.org/getting-started/basic-types.html#atoms)
  * [Atom Part 2](https://elixirschool.com/en/lessons/basics/basics/#atoms)
  * [Sigils](https://inquisitivedeveloper.com/lwm-elixir-41/)
  * [The Kernel Module Part 1](https://inquisitivedeveloper.com/lwm-elixir-50/)
  * [The Kernel Module Part 2](https://inquisitivedeveloper.com/lwm-elixir-53/)
  * [The Kernel Module Part 3](https://inquisitivedeveloper.com/lwm-elixir-54/)
  * [The Kernel Module Part 4](https://inquisitivedeveloper.com/lwm-elixir-62/)
  * [The Kernel Module Part 5](https://inquisitivedeveloper.com/lwm-elixir-68/)
  * [Default Values and Multiple Function Clauses](https://inquisitivedeveloper.com/lwm-elixir-25/)
  * [Pattern Matching](https://inquisitivedeveloper.com/lwm-elixir-22/)
  * [Functions and Pattern Matching](https://inquisitivedeveloper.com/lwm-elixir-23/)
  * [Composing Functions](https://inquisitivedeveloper.com/lwm-elixir-29/)
  * [A community driven style guide for Elixir](https://github.com/christopheradams/elixir_style_guide)
  * [Credo - A static code analysis tool for the Elixir](https://github.com/rrrene/credo)

For the second lesson, it will develop the main module to our game, called Tetromino. It's our public API.

The module will be its only data type use struct, and a static constructor to guarantee the same data for the struct and giving another constructor to random the data.

The Tetromino, will be:
  * ___shape___ - _:i, :j, :l, :o, :z, :s, :t_.
  * ___rotation___ - _0 90 180 270_.
  * ___location___ - using the cartisian plan and the module point to change the current location.

And we will see the differences between structs and maps, and they're on particularities to interact with their data types and transform the data.

___HINT:___ using the `v()` IEx Command to access some particular return in the IEx history lines of execution.

The Tetromino will have they on reducers functions to change the data and if it is necessary using as like a `factory process`: input -> transformations -> output, and pass to the next step doing your computations in the data.

#### IEX Session
```elixir
$ iex -S mix
Erlang/OTP 23 [erts-11.1.1] [source] [64-bit] [smp:12:12] [ds:12:12:10] [async-threads:1] [hipe]

Interactive Elixir (1.11.1) - press Ctrl+C to exit (type h() ENTER for help)
iex[1]> alias Tetris.Tetromino
Tetris.Tetromino
iex[2]> Tetromino.__struct__  
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}
iex[3]> Tetromino.__struct__ |> Map.keys
[:__struct__, :location, :rotation, :shape]
iex[4]> %{location: {5, 1}, rotation: 0, shape: :l}                
%{location: {5, 1}, rotation: 0, shape: :l}
iex[5]> %{location: {5, 1}, rotation: 0, shape: :l} |> Map.keys
[:location, :rotation, :shape]
iex[6]> l = v(2)
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}
iex[7]> l |> Map.keys
[:__struct__, :location, :rotation, :shape]
iex[8]> l.__struct__
Tetris.Tetromino
iex[9]> l.bad_key
** (KeyError) key :bad_key not found in: %Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}

iex[9]> %{l | locatoin: {10, 10}}
** (KeyError) key :locatoin not found in: %Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}. Did you mean one of:

      * :location

    (stdlib 3.13.2) :maps.update(:locatoin, {10, 10}, %Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l})
    (stdlib 3.13.2) erl_eval.erl:259: anonymous fn/2 in :erl_eval.expr/5
    (stdlib 3.13.2) lists.erl:1267: :lists.foldl/3
iex[9]> Tetromino.__struct__
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}
iex[10]> Tetromino.__struct__(shape: :o)
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :o}
iex[11]> recompile
Compiling 1 file (.ex)
:ok
iex[12]> Tetromino.new
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}
iex[13]> Tetromino.new(location: {10, 10})
%Tetris.Tetromino{location: {10, 10}, rotation: 0, shape: :l}
iex[14]> Tetromino.new(location: {10, 10}, shape: :t)
%Tetris.Tetromino{location: {10, 10}, rotation: 0, shape: :t}
iex[15]> [:a, :b, :c]
[:a, :b, :c]
iex[16]> ~w(a b c)
["a", "b", "c"]
iex[17]> ~w(a b c)a
[:a, :b, :c]
iex[18]> ~w[a b c]a
[:a, :b, :c]
iex[19]> ~w/a b c/a
[:a, :b, :c]
iex[20]> ~w(i t o l j z s)a
[:i, :t, :o, :l, :j, :z, :s]
iex[21]> i
Term
  [:i, :t, :o, :l, :j, :z, :s]
Data type
  List
Reference modules
  List
Implemented protocols
  Collectable, Enumerable, IEx.Info, Inspect, Jason.Encoder, List.Chars, Phoenix.HTML.Safe, Phoenix.Param, Plug.Exception, String.Chars
iex[22]> i ~w(i t o l j z s)a
Term
  [:i, :t, :o, :l, :j, :z, :s]
Data type
  List
Reference modules
  List
Implemented protocols
  Collectable, Enumerable, IEx.Info, Inspect, Jason.Encoder, List.Chars, Phoenix.HTML.Safe, Phoenix.Param, Plug.Exception, String.Chars
iex[23]> exports Enum
all?/1                all?/2                any?/1                any?/2                
at/2                  at/3                  chunk/2               chunk/3               
chunk/4               chunk_by/2            chunk_every/2         chunk_every/3         
chunk_every/4         chunk_while/4         concat/1              concat/2              
count/1               count/2               dedup/1               dedup_by/2            
drop/2                drop_every/2          drop_while/2          each/2                
empty?/1              fetch/2               fetch!/2              filter/2              
filter_map/3          find/2                find/3                find_index/2          
find_value/2          find_value/3          flat_map/2            flat_map_reduce/3     
frequencies/1         frequencies_by/2      group_by/2            group_by/3            
intersperse/2         into/2                into/3                join/1                
join/2                map/2                 map_every/3           map_intersperse/3     
map_join/2            map_join/3            map_reduce/3          max/1                 
max/2                 max/3                 max_by/2              max_by/3              
max_by/4              member?/2             min/1                 min/2                 
min/3                 min_by/2              min_by/3              min_by/4              
min_max/1             min_max/2             min_max_by/2          min_max_by/3          
min_max_by/4          partition/2           random/1              reduce/2              
reduce/3              reduce_while/3        reject/2              reverse/1             
reverse/2             reverse_slice/3       scan/2                scan/3                
shuffle/1             slice/2               slice/3               sort/1                
sort/2                sort_by/2             sort_by/3             split/2               
split_while/2         split_with/2          sum/1                 take/2                
take_every/2          take_random/2         take_while/2          to_list/1             
uniq/1                uniq/2                uniq_by/2             unzip/1               
with_index/1          with_index/2          zip/1                 zip/2                 
iex[24]> exports List
ascii_printable?/1     ascii_printable?/2     delete/2               delete_at/2            
duplicate/2            first/1                flatten/1              flatten/2              
foldl/3                foldr/3                improper?/1            insert_at/3            
keydelete/3            keyfind/3              keyfind/4              keymember?/3           
keyreplace/4           keysort/2              keystore/4             keytake/3              
last/1                 myers_difference/2     myers_difference/3     pop_at/2               
pop_at/3               replace_at/3           starts_with?/2         to_atom/1              
to_charlist/1          to_existing_atom/1     to_float/1             to_integer/1           
to_integer/2           to_string/1            to_tuple/1             update_at/3            
wrap/1                 zip/1                  
iex[25]> recompile
Compiling 1 file (.ex)
:ok
iex[26]> Tetromino.new_random()
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :t}
iex[27]> Tetromino.new_random()
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :j}
iex[28]> Tetromino.new_random()
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :i}
iex[29]> tetro = Tetromino.new()
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}
iex[30]> %{tetro | location: {6, 1}}
%Tetris.Tetromino{location: {6, 1}, rotation: 0, shape: :l}
iex[31]> tetro
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}
iex[32]> recompile
Compiling 1 file (.ex)
:ok
iex[33]> tetro
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}
iex[34]> tetro |> Tetromino.right()
%Tetris.Tetromino{location: {6, 1}, rotation: 0, shape: :l}
iex[35]> tetro |> Tetromino.right() |> Tetromino.down()
%Tetris.Tetromino{location: {6, 2}, rotation: 0, shape: :l}
iex[36]> recompile
Compiling 1 file (.ex)
warning: variable "tetro" is unused (if the variable is not meant to be used, prefix it with an underscore)
  lib/tetris/tetromino.ex:26: Tetris.Tetromino.rotate/1

:ok
iex[37]> Tetromino.rotate
rotate/1            rotate_degrees/1    
iex[38]> Tetromino.rotate_degrees(0)
90
iex[39]> Tetromino.rotate_degrees(0) |> Tetromino.rotate_degrees()
180
iex[40]> Tetromino.rotate_degrees(0) |> Tetromino.rotate_degrees() |> Tetromino.rotate_degrees()
270
iex[41]> Tetromino.rotate_degrees(0) |> Tetromino.rotate_degrees() |> Tetromino.rotate_degrees() |> Tetromino.rotate_degrees()
0
iex[42]> Tetromino.rotate_degrees(270)
0
iex[43]> recompile
Compiling 1 file (.ex)
:ok
iex[44]> tetro
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}
iex[45]> tetro |> Tetromino.rotate()
%Tetris.Tetromino{location: {5, 1}, rotation: 90, shape: :l}
iex[46]> tetro |> Tetromino.rotate() |> Tetromino.rotate()
%Tetris.Tetromino{location: {5, 1}, rotation: 180, shape: :l}
iex[47]> tetro |> Tetromino.rotate() |> Tetromino.rotate() |> Tetromino.rotate()
%Tetris.Tetromino{location: {5, 1}, rotation: 270, shape: :l}
iex[48]> tetro |> Tetromino.rotate() |> Tetromino.rotate() |> Tetromino.rotate() |> Tetromino.rotate()
%Tetris.Tetromino{location: {5, 1}, rotation: 0, shape: :l}
```
