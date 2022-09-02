# Basics
## Basics

Using iex command (or iex.bat for windows), we can directly execute elixir on terminal.

Examples:

```
iex> 2+3
5
iex> 2+3 == 5
true
iex> String.length("The quick brown fox jumps over the lazy dog")
43
```

### Basic Data Types
* Integers
    * Support for binary, octal, and hexadecimal numbers comes built in
* Floats
    * In Elixir, floating point numbers require a decimal after at least one digit; they have 64-bit double precision and support e for exponent values
* Booleans
    * true
    * false
    * nil
* Atoms
    * An atom is a constant whose name is its value
    * true and false are also :true and :false
    * Names of modules in Elixir are also atoms
    * Atoms are also used to reference omdules for Erlang libraries, including built in ones
* Strings
    Strings in Elixir are UTF-8 encoded and are wrapped in double quotes

### Basic Operations
* Arithmetic
    * +, -, *, /
    * div(), rem()
* Boolean
    * ||, &&, !
    * and, or, not
* Comparison
    * ==, !=, ===, !==, <=, >=, <, >
    * number < atom < reference < function < port < pid < tuple < map < list < bitstring
* String Interpolation
    * "blablabla #{interpolation_value}"
* String Concatenation
    * "Concat1" <> concat_2

## [Collections](#collections)
### Lists
List are simple collections of values which may include multiple types
Whe can prepend (fast)
```
iex> ["π" | list]
["π", 3.14, :pie, "Apple"]
```
or append (slow)
```
iex> list ++ ["Cherry"]
[3.14, :pie, "Apple", "Cherry"]
```
#### List Concatenation
List concatenation uses the ++ operator

#### List Subtraction
Uses the -- operator

#### Head / Tail
We can use hd and tl for working with the first item (hd) and the rest (tl) of the list
```
iex> hd [3.14, :pie, "Apple"]
3.14
iex> tl [3.14, :pie, "Apple"]
[:pie, "Apple"]
```
We can also use | operator to split this items
```
iex> [head | tail] = [3.14, :pie, "Apple"]
[3.14, :pie, "Apple"]
iex> head
3.14
iex> tail
[:pie, "Apple"]
```

### Tuples
Similar to lists, but stored contiguously in memory. This makes accessing their length fast but modification expensive

### Keyword lists
Keyword lists and maps are the associative collections of Elixir.
The three characteristics of keyword lists highlight their importance:
* Keys are atoms.
* Keys are ordered.
* Keys do not have to be unique.

### Maps
In Elixir maps are the "go-to" key value store, allowing keys of any type and un-ordered

## [Enum](#enum)
A set of algorithms for enumeration over enumerables.

### Common Functions
* all?
    * Returns true if all elements of the enumerable are true
* any?
    * Returns true if any element of the enumerable is true
* chunk_every
    * Returns a list of lists of size n
* chunk_by
    * Returns a list of lists of elements that are equal to the current element
* map_every
    * Returns a list of the results of applying the function to each element of the enumerable that matches to the nth value
* each
    * Iterates over the collection without producing a new value
* map
    * Returns a list of the results of applying the function to each element of the enumerable
* min
    * Returns the minimal value in the enumerable
* max
    * Returns the maximal value in the enumerable
* filter
    * Returns a list of the elements of the enumerable for which the function returns true
* reduce
    * Returns the result of applying the function to the elements of the enumerable
* sort
    * Returns a sorted list of the elements of the enumerable
* uniq
    * Returns a list of the unique elements of the enumerable
* uniq_by
    * Also removes duplicates from enumerables, but it allows us to provide a function to do the uniqueness comparison.

### Enum using the Capture operator (&)
We can use & as a shorthand to anonymous functions:
```
iex> Enum.map([1,2,3], &(&1 + 3))
[4, 5, 6]
```
Or use this in named functions like that:
```ex
defmodule Adding do
    def plus_three(number), do: number + 3
end

iex> Enum.map([1,2,3], &Adding.plus_three(&1))
[4, 5, 6]
```

## [Pattern Matching](#pattern-matching)
Pattern matching allows us to match simple values, data structures, and even functions.

### Match Operator
In Elixir, the = operator is actually a match operator:
```
iex> x = 1
1
iex> 1 = x
1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```
### Pin Operator
When we pin a variable we match on the existing value rather than rebinding to a new one. Let’s take a look at how this works:
```
iex> x = 1
1
iex> ^x = 2
** (MatchError) no match of right hand side value: 2
iex> {x, ^x} = {2, 1}
{2, 1}
iex> x
2
```

## [Control Structures](#control-structures)
### if and unless
In Elixir, the only falsey values are nil and the boolean false.
Using unless is like if only it works on the negative.

### case
If it's necessary to match against multiple patterns we can use case.
With the _ variable we can define default action if the cases doesn't match.
If you intend to match against existing variables you must use the ^ operator

### cond
When we need to match conditions rather than values we can turn to cond. This is akin to else if or elsif from other languages

### with
The with expression is composed of the keywords, the generators, and finally an expression:
```
iex> user = %{first: "Sean", last: "Callan"}
%{first: "Sean", last: "Callan"}
iex> with {:ok, first} <- Map.fetch(user, :first),
...>     {:ok, last} <- Map.fetch(user, :last),
...>     do: last <> ", " <> first
"Callan, Sean"
```

## [Functions](#functions)
### Anonymous Functons
When we talk about anonymous functions in Elixir we need the `fn` and `end` keywords. Within these we can define any number of parameters and function bodies separated by ->
```
iex> sum = fn (a, b) -> a + b end
iex> sum.(2, 3)
5
```
We can use & shorthand too
```
iex> sum = &(&1 + &2)
iex> sum.(2, 3)
5
```

### Pattern Matching
Pattern matching isn’t limited to just variables in Elixir, it can be applied to function signatures as we will see in this section.
Elixir uses pattern matching to check through all possible match options and select the first matching option to run:
```
iex> handle_result = fn
...>   {:ok, result} -> IO.puts "Handling result..."
...>   {:ok, _} -> IO.puts "This would be never run as previous will be matched beforehand."
...>   {:error} -> IO.puts "An error has occurred!"
...> end

iex> some_result = 1
1
iex> handle_result.({:ok, some_result})
Handling result...
:ok
iex> handle_result.({:error})
An error has occurred!
:ok
```

### Named Functions
We can define functions with names so we can easily refer to them later. Named functions are defined within a module using the `def` keyword.
If our function body only spans one line, we can shorten it further with `do:`

### Function Naming and Arity
We mentioned earlier that functions are named by the combination of giver name and arity. This means we can declare diferent functions with the same name by changing their arity.

### Functions and Pattern Matching
Behind the scenes, functions are pattern-matching the arguments that they’re called with.
Say we needed a function to accept a map but we’re only interested in using a particular key. We can pattern-match the argument on the presence of that key like this:
```ex
defmodule Greeter1 do
    def hello(%{name: person_name}) do
    IO.puts "Hello, " <> person_name
    end
end
```

### Private Functions
Private functions can only be called from within their own Module. We define them in Elixir with `defp`.

### Guards
In Elixir we can use `when` keyword to execute diferent functions with the same name, like this:
```ex
defmodule Greeter do
    def hello(names) when is_list(names) do
        names = Enum.join(names, ", ")
        
        hello(names)
    end

    def hello(name) when is_binary(name) do
        phrase() <> name
    end

    defp phrase, do: "Hello, "
end

iex> Greeter.hello ["Sean", "Steve"]
"Hello, Sean, Steve"
```

### Default Arguments
We can define default arguments with `argument \\ value` styntax.

## [Pipe Operator](#pipe)
### Introduction
The pipe (`|>`) operator takes the result on the left, and passes it to the right hand side.
### Example
```
iex> "Elixir rocks" |> String.split()
["Elixir", "rocks"]
```

## [Modules](#modules)
### Modules
Modules allow us to organize functions into a namespace. In addition to grouping functions, they allow us to define named and private functions.

### Module Attributes
We can define atributes inside modules with `@`.
There are 3 most common reserved attributes in Elixir:
- moduledoc — Documents the current module.
- doc — Documentation for functions and macros.
- behaviour — Use an OTP or user-defined behaviour.

### Structs
Structs are special maps with a define set of keys and default values. A struct must be defined within a module, which it takes its name from. It is common for a struct to be the only thing defined within a module;
To define it we use `defstruct` along with a keyword list of fields and default values:
```ex
defmodule Example.User do
    defstuct name: "Sean", roles: []
end
```
Let's create some structs:
```
iex> %Exampl.User{}
%Example.User<name: "Sean", roles [], ...>
iex> %ExampleUser{name: "Steve"}
%Example.User{name: "Steve", roles: []}
iex> %ExampleUser{name: "Steve", roles: [:manager]}
%Example.User{name: "Steve", roles: [:manager]}
```
All of our fields are present which is okay for this example but what if we had a protected field we didn't want to include? The new `@derive` feature let's us accomplish just this! Let's update our example so `roles` are no longer included in our output:
```ex
defmodule Example.User do
    @derive { Inspect, only: [:name]}
    defstruct name: nil, roles []
end
```

### Composition
Compositions allow us to add existing functionalities, Elixir provides us with a variety of different ways to interact with other modules.

#### alias
Allows us to alias module names; used quite frequently in Elixir code:
```
defmodule Sayings.Greetings do
    def basic(name), do: "Hi, #{name}"
end

defmodule Example do
    alias Sayings.Greetings

    def greeting(name), do: Greetings.basic(name)
end

# Without alias

defmodule Example do
    def greeting(name), do: Sayings.Greetings.basic(name)
end
```
And we can use `:as` option to name them differently.
```ex
defmodule Example do
    alias Sayings.Greetings, as: Hi

    def print_message(name), do: Hi.basic(name)
end
```
It’s even possible to alias multiple modules at once:
```ex
defmodule Example do
    alias Sayings.{Greetings, Farewells}
end
```

---
`import`

If we want to import functions rather than aliasing the module we can use `import`

|> filtering

By default all functions and macros are imported but we can filter them using the `:only` and `:except` options.
To import specific functions and macros, we must provide the name/arity pairs to `:only` and `:except`. Let's start by importing only the `last/1` function:
In addition to the name/arity pairs there are two special atoms, :functions and :macros, which import only functions and macros respectively:
```ex
import List, only: :functions
import List, only: :macros
```
---

`require`

We could use `require` to tell Elixir you're going to use macros from another module. The slight difference with `import` is that it allows using macros, but not functions from the specified module.

```ex
defmodule Example do
    require SuperMacros

    SuperMacros.do_stuff
end
```

---

`use`

With the `use` macro we can enable another module to modify our current module's definition. When we call `use` in our code we're actually invoking the `__using__/1` callback defined by the providaded module. The result of the `__using__/1` macro becomes part of our module's definition. To get a better understanding how this works let's look at a simple example:

```ex
defmodule Hello do
    defmacro __using__(_opts) do
        quote do
            def hello(name), do: "Hi, #{name}"
        end
    end
end
```
Here we've created a `Hello` module that defines the `__using__/1` callback inside of which we define a `hello/1` function. Let's create a new module so we can try out new code:
```ex
defmodule Example do
    use Hello
end
```
Here we can see that `use` invoked the `__using__/1` callback on `Hello` which in turn added the resulting code to our module. Now that we’ve demonstrated a basic example let’s update our code to look at how `__using__/1` supports options. We’ll do this by adding a `greeting` option:
```ex
defmodule Hello do
    defmacro __using__(opts) do
        greeting = Keyword.get(opts, :greeting, "Hi")

        quote do
            def hello(name), do: unquote(greeting) <> ", " <> name
        end
    end
end
```
Let’s update our `Example` module to include the newly created `greeting` option:
```ex
defmodule Example do
    use Hello, greeting: "Hola"
end
```
If we give it a try in IEx we should see that the greeting has been changed:
```
Example.hello("Sean")
"Hola, Sean"
```

## [Mix](#mix)
### New Projects
When we're ready to create a new Elixir project, Mix makes it easy with the `mix new` command. This will generate our project's folder structure and necessary boilerplate.

### Interactive
It may be necessary to use `iex` within the context of our application. Thankfully for us, Mix makes thiseasy. We can start a new `iex` session:
```
cd example
iex -S mix
```
Starting `iex` this way will load your application and dependencies into the current runtime.

### Compilation
Mix is smart and will compile your changes when necessary, but it may still be necessary to explicitly compile your project. In this section we'll cover how to compile our project and what compilation does.
To compile a Mix project we only need to run `mix compile` in our base directory.
```
mix compile
```
When we compile a project, Mix creates a `_build` directory for our artifacts. If we look inside `_build` we will see our compiled application: `example.app`.

### Managing Dependencies
Our project doesn't have any dependencies but will shortly, so we'll go ahead and cover defining dependencies and fetching them.
To add a new dependency we need to first add it to our `mix.exs` in the `deps` section. Our dependendcy list is comprised of tuples with two required values and one optional: the package name as an atom, the version string, and optional options.
For this example let's look at a project with dependencies, like `phoenix slim`:
```ex
def deps do
    [
        {:phoenix, "~> 1.1 or ~> 1.2"},
        {:phoenix_html, "~> 2.3"},
        {:cowboy, "~> 1.0", only: [:dev, :test]},
        {:slime, "~> 0.14"}
    ]
end
```
As you probably discerned from the dependencies aboce, the `cowboy` dependency is only necessary during development and test.
Once we’ve defined our dependencies there is one final step: fetching them. This is analogous to `bundle install`:
```
mix deps.get
```
### Environments
Mix, much like Bundler, supports differing environments. Out of the box Mix is configured to have three environments:
- `:dev` - The default environment.
- `:test` - Used by `mix test`. Convered further in our next lesson.
- `:prod` - Used when we ship our application to production.
The current environment can be accessed using `Mix.env`. As expected, the environment can be changed via the `MIX_ENV` environment variable:
```
MIX_ENV=prod mix compile
```

## [Sigils](#sigils)
### Introduction
Elixir provides an alternative syntax for representing and working with literals. A sigil will start with a tilde `~` followed by a character. The Elixir core provides us with somebuilt in sigils however, it is possile to create our own when we need to extend the language.
A list of available sigils include:
- `~C` Generates a character list with no escaping or interpolation
- `~c` Generates a character list with escaping and interpolation
- `~R` Generates a regular expression with no escaping or interpolation
- `~r` Generates a regular expression with escaping and interpolation
- `~S` Generates a string with no escaping or interpolation
- `~s` Generates a string with escaping and interpolation
- `~W` Generates a word list with no escaping or interpolation
- `~w` Generates a word list with escaping and interpolation
- `~N` Generates a `NativeDateTime` struct
- `~U` Generates a `DateTime` struct
A list of delimiters include:
- `<...>` A pair of pointy brackets
- `{...}` A pair of curly brackets
- `[...]` A pair of square brackets
- `(...)` A pair of parentheses
- `|...|` A pair of pipes
- `/.../` A pair of forward slashes
- `"..."` A pair of double quotes
- `'...'` A pair of single quotes

### Char List
The `~c` and `~C` sigils generate character lists respectively. For example:
```
iex> ~c/2 + 7 = #{2 + 7}/
'2 + 7 = 9'
iex> ~C/2 + 7 = #{2 + 7}/
'2 + 7 = \#{2 + 7}'
```

### Regular Expressions
The `~r` and `~R` sigils are used to represent Regular Expressions. We create them either on the fly or for use within the `Regex` functions. For example:
```
iex> re = ~r/elixir/
~r/elixir/
iex> "Elixir" =~ re
false
iex> "elixir" =~ re
true
```

### String
The `~s` and `~S` sigils are used to generate string data. For example:
```
iex> ~s/welcome to elixir #{String.downcase "SCHOOL"}/
"welcome to elixir school"
iex> ~S/welcome to elixir #{String.downcase "SCHOOL"}/
"welcome to elixir \#{String.downcase \"SCHOOL\"}"
```

### Word List
The word list sigil can come in handy from time to time. It can save both time, keystrokes and arguably reduce the complexity within the codebase. Take this simple example:
```
iex> ~w/i love #{'e'}lixir school/
["i", "love", "elixir", "school"]
iex> ~W/i love #{'e'}lixir school/
["i", "love", "\#{'e'}lixir", "school"]
```

### NaiveDateTime
For the most part, we should avoid creating a `NaiveDateTime` struct directly. However, it is useful for pattern matching. For example:
```
iex> NaiveDateTime.from_iso8601("2015-01-23 23:50:07") == {:ok, ~N[2015-01-23 23:50:07]}
```

### DateTime
A `DateTime` can be useful for quickly creating a struct to represent a `DateTime` with a UTC timezone. Since it's in the UTC timezone and your string might represent a different timezone, a 3rd item is returned that represents the offset in seconds.
For example:
```
iex> DateTime.from_iso8601("2015-01-23 23:50:07Z") == {:ok, ~U[2015-01-23 23:50:07Z], 0}
iex> DateTime.from_iso8601("2015-01-23 23:50:07-0600") == {:ok, ~U[2015-01-24 05:50:07Z], -21600}
```

### Creating Sigils
One of the goals of Elixir is to be an extendable programming language. It should come as no surprise then that you can easily create your own custom sigils. In this example, we will create a sigil to convert a string to uppercase. As there is already a function for this in the Elixir Core `(String.upcase/1)`, we will wrap our sigil around that function.
```
iex> defmodule MySigils do
...>     def sigil_p(string, []), do: String.upcase(string)
...> end
iex> import MySigils
nil
iex> ~p/elixir school/
ELIXIR SCHOOL
```

## Documentation
### Annotation
How much we commented and what makes quality documentation remains a contentious issue within the programming world. However, we can all agree that documentation is important for ourselves and those working with our codebase.
Elixir trets doumentation as a first-class citizen, offering various functions to access and generate documentation for your projects. The Elixir core provides us with many different attributes to annotate a codebase. Let's look at 3 ways:
- `#` - For inline documentation.
- `@moduledoc` - For module-level documentation.
- `@doc` - For function-level documentation.

### Inline Documentation
Probably the simplest way to comment your code is with inline comments. Similar to Ruby or Python, Elixir's inline comment is denoted with a `#`, frequently known as a pound, or a hash depending on where your are from in the world.
Take this Elixir Script (greeting.exs):
```ex
# Outputs 'Hello, chum.' to the console.
IO.puts("Hello, " <> "chum.")
```

### Documenting Modules
The `@moduledoc` annotator allows for inline documentation at a modul level. It typically sits just under the `defmodule` declaration at the top of a file. The example below shows a one line comment within the `@moduledoc` decorator.
```ex
defmodule Greeter do
    @moduledoc """
    Provides a function `hello/1` to greet a human
    """

    def hello(name) do
        "Hello, " <> name
    end
end
```
We (or others) can access this module documentation using the `h` helper function within IEx. We can see this for ourselves if we put our `Greeter` module into a new file, `greeter.ex` and compile it.

### Document Functions
Just as Elixir gives us the ability for module level annotation, it also enables similar annotations for documenting functions. The `@doc` annotator allows for inline documentation at a function level. The `@doc` annotator sits just above the function it is annotating.
```ex
defmodule Greeter do
    @moduledoc """
    ...
    """

    @doc """
    Prints a hello message.

    ## Parameters

        - name: String that represents the name of the person.

    ## Examples

        ```
        iex> Greeter.hello("Sean")
        "Hello, Sean"

        iex> Greeter.hello("pete")
        "Hello, pete"
        ```
    """
    @spec hello(String.t()) :: String.t()
    def hello(name) do
        "Hello, " <> name
    end
end
```

### ExDoc
ExDoc is an official Elixir project that produces HTML and online documentation for Elixir projects.
We can install it doing like this on `mix.exs`:
```ex
    def deps do
        [{:ex_doc, "~> 0.21", only: :dev, runtime: false}]
    end
```
To generate a documentation we run the following to commands:
```
$ mix deps.get # gets ExDoc + Earmark.
$ mix docs # makes the documentation.
```

### Best Pratice
Documentation should be added within the Best Pratices Guidelines of the language. Since Elixir is a fairly young language many standards are still to be discovered as the ecosystem grows. The community, however, tried to establish best pratices.
- Always document a module.
```
defmodule Greeter do
    @moduledoc """
    This is good documentation.
    """
end
```
- If you do not intent to document a module, do not leave it blank. Consider annotating the module `false`, like so:
```
defmodule Greeter do
    @moduledoc false
end
```
- When referring to functions within module documentation, use backticks like so:
```
defmodule Greeter do
    @moduledoc """
    ...

    This module also has a `hello/1` function.
    """

    def hello(name) do
        IO.puts("Hello, " <> name)
    end
end
```
- Separate any and all code one line under de `@moduledoc as so:`
```ex
defmodule Greeter do
    @moduledoc """
    ...

    This module also has a `hello/1` function.
    """

    alias Goodbye.bye_bye
    # and so on...

    def hello(name) do
        IO.puts("Hello, " <> name)
    end
end
```
- Use Markdown within docs. It will make it easier to read either via IEx or ExDoc.
```ex
defmodule Greeter do
    @moduledoc """
    ...
    """

    @doc """
    Prints a hello message

    ## Parameters

        - name: String that represents the name of the person.

    ## Examples

        iex> Greeter.hello("Sean")
        "Hello, Sean"

        iex> Greeter.hello("pete")
        "Hello, pete"

    """
    @spec hello(String.t()) :: String.t()
    def hello(name) do
        "Hello, " <> name
    end
end
```

## Comprehensions
### Basics
Comprehensions can often be used to produce more concise statements for `Enum` and `Stream` iteration. Let's start by looking at a simple comprehension and then break it down:
```
iex> list = [1, 2, 3, 4, 5]
iex> for x <- list, do: x*x
[1, 4, 9, 16, 25]
```
The first thing we notice is the use of `for` and a generator. What is a generator? Generators are the `x <- [1, 2, 3, 4]` expressions found in list comprehensions. They're responsible for generating the next value.
Lucky for us, comprehensions aren't limited to lists; in fact they'll work with any enumerable:
```
# Keyword Lists
iex> for {_key, val} <- [one: 1, two: 2, three: 3], do: val
[1, 2, 3]

# Maps
iex> for {k, v} <- %{"a" => "A", "b" => "B"}, do: {k, v}
[{"a", "A"}, {"b", "B"}]

# Binaries
iex> for <<c <- "hello">>, do: <<c>>
["h", "e", "l", "l", "o"]
```
Like many other things in Elixir, generators rely on pattern matching to compare their input set to the left side variable. In the event a match is not found, the value is ignored:
```
iex> for {:ok, val} <- [ok: "Hello", error: "Unknown", ok: "World"], do: val
["Hello", "World"]
```
It’s possible to use multiple generators, much like nested loops:
```
iex> list = [1, 2, 3, 4]
iex> for n <- list, times <- 1..n do
...>   String.duplicate("*", times)
...> end
["*", "*", "**", "*", "**", "***", "*", "**", "***", "****"]
```
### Filters
You can think of filters as a sort of guard for comprehensions. When a filtered value returns `false` or `nil` it is excluded from the final list. Let's loop over a range and only worry about even numbers. We'll use the `is_even/1` function from the Integer module to check if a value is value is even or not.
```
import integer
iex> for x <- 1..10, is_even(x), do: x
[2, 4, 6, 8, 10]
```
Like generators, we can use multiple filters. Let’s expand our range and then filter only for values that are both even and evenly divisible by 3.
```
import Integer
iex> for x <- 1..100,
...>   is_even(x),
...>   rem(x, 3) == 0, do: x
[6, 12, 18, 24, 30, 36, 42, 48, 54, 60, 66, 72, 78, 84, 90, 96]
```
### Using :into
What if we want to produce something other than a list? Given the `:into` option we can do just that! As a general rule of thumb. `:into` accepts any structure that implements the `Collectable` protocol.
Using `:into` let's create a map from a keyword list:
```
iex> for {k, v} <- [one: 1, two: 2, three: 3], into: %{}, do: {k, v}
%{one: 1, three: 3, two: 2}
```
Since binaries are collectables we can use list comprehensions and `:into` to create strings:
```
iex> for c <- [72, 101, 108, 108, 111], into: "", do: <<c>>
"Hello"
```