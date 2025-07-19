# ***wren.c3l***

"wren.c3l" is a [C3][c3] library that contains bindings to the [Wren][wren]
programming language.

This library allows you to easily embed Wren into C3 applications.

You can find the original repository for Wren [here][wren-git].
Below is the README from the original project.

## Introduction

### Wren is a small, fast, class-based concurrent scripting language

Think smalltalk in a Lua-sized package with a dash of Erlang and wrapped up
in a familiar, modern [syntax](https://wren.io/syntax.html).

```dart
System.print("Hello, world!")

class Wren {
  flyTo(city) {
    System.print("Flying to %(city)")
  }
}

var adjectives = Fiber.new {
  ["small", "clean", "fast"].each {|word| Fiber.yield(word) }
}

while (!adjectives.isDone) System.print(adjectives.call())
```

* **Wren is small.** The VM implementation is under [4,000 semicolons][src].
    You can skim the whole thing in an afternoon. It's *small*, but not
    *dense*. It is readable and [lovingly-commented][nan].

* **Wren is fast.** A fast single-pass compiler to tight bytecode, and a
    compact object representation help Wren [compete with other dynamic
    languages][perf].

* **Wren is class-based.** There are lots of scripting languages out there,
    but many have unusual or non-existent object models. Wren places
    [classes][] front and center.

* **Wren is concurrent.** Lightweight [fibers][] are core to the execution
    model and let you organize your program into an army of communicating
    coroutines.

* **Wren is a scripting language.** Wren is intended for embedding in
    applications. It has no dependencies, a small standard library,
    and [an easy-to-use C API][embedding]. It compiles cleanly as C99, C++98
    or anything later.

## Usage

To use this library, clone the repository into your 'lib' directory for your
C3 project:

```
git clone https://github.com/Utecha/wren.c3l.git
cd wren.c3l
```

Wren is no longer really being developed on, and the only real way to use it
is through the original repository.

As a result, the original repository is included as submodule in order to gain
access to 'libwren'. To get the submodule, run this command:

```
git submodule update --init
```

Next step is to build Wren itself. Wren uses [Premake][premake] to generate project
files for different compilers and platforms.

### Build on Windows

Open the `wren.sln` inside of `projects/vs2019` in Visual Studio and run the
build.

### Build on Linux/BSD

For Linux run:

```
cd wren
make -C projects/make
```

For BSD, instead run:

```
cd wren
make -C projects/make.bsd
```

### Build for MacOS

If you use XCode, open the `wren.xcworkspace` inside of the `projects/xcode/` directory
and run the build.

If you have/prefer Make, there is also a make project for Mac:

```
make -C projects/make.mac
```

---

Once Wren has finished building, all that's left to do is make a couple of
adjustments to your `project.json`.

The `manifest.json` inside of C3 libraries does not have access to the
`linker-search-paths` property that the `project.json` does for executable C3
projects. As a result, you need to add it to your `project.json` in order
for `wren.c3l` to be able to find `libwren.a` or `libwren.so`:

```json
// This should exist in your `project.json` by default, so just add `wren` to it.
"dependencies": [ "wren" ],

// Add this into your `project.json`
"linker-search-paths": [ "lib/wren.c3l/wren/lib" ]
```

`wren.c3l` already *attempts* to link to `libwren`, so there's no need to add it
to the `linked-libraries` property.
Your main project only needs to provide the linker search path so that it
*can* properly link.

And you're done! At this point you can now `import wren` in your code and start
using the library!

---

[c3]: https://c3-lang.org
[wren]: https://wren.io/
[wren-git]: https://github.com/wren-lang/wren
[premake]: https://premake.github.io/
[src]: https://github.com/wren-lang/wren/tree/main/src
[nan]: https://github.com/wren-lang/wren/blob/93dac9132773c5bc0bbe92df5ccbff14da9d25a6/src/vm/wren_value.h#L486-L541
[perf]: http://wren.io/performance.html
[classes]: http://wren.io/classes.html
[fibers]: http://wren.io/concurrency.html
[embedding]: http://wren.io/embedding/
