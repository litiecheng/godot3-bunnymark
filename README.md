![Godot Bunnymark](images/banner.png)

Renders an increasing number of bunny sprites until a stable 60fps is hit.  This is a decent test of real world usage as it combines Godot api usage with raw computation.  I plan to update this README whenever significant performance changes occur or when new languages are added.  Feel free to contribute language implementations or improvements!

## Disclaimer

It is important to note that C#/Mono and GDNative are both very young.  Its possible that their performance characteristics will change.  Additionally, these are just a few benchmarks testing a few use cases.  Please don't use them to say "Language X is better / faster than Language Y", we don't have enough data to make those assertions.  If anything this proves that any of the choices below are viable.  Choose the language that you are comfortable with and do your own testing to cover your own scenarios.

## Running

* Build C++ files
    * Setup headers and bindings using [these directions](https://github.com/GodotNativeTools/godot-cpp)
    * run ```make``` in the root of this project
* Build C# files
    * run ```msbuild /p:Configuration=Tools;DebugSymbols=false;Optimize=true``` (some terminals might require escaping some of those symbols)
* Build Nim files
    * Setup headers and bindings using [these directions](https://pragmagic.github.io/godot-nim/master/index.html)
    * run ```nake build```
* Build D files
    * `git clone` [godot-d](https://github.com/GodotNativeTools/godot-d) to your favorite directory
    * [generate the bindings](https://github.com/GodotNativeTools/godot-d/blob/master/generator/README.md)
    * run `dub add-local /path/to/godot-d/`
    * run `dub build -b release`
* run ```sh run_benchmarks.sh```
* wait!  This will take some time ... the automation code is still a bit naive so it takes awhile to converge on 60 fps
* view the results in ```USER_HOME_DIRECTORY/.godot/app_userdata/Bunnymark/benchmark_results.json```

## Benchmark Run - February 22, 2018

### BunnymarkV2

Attempts to draw as many sprites as possible using Sprite nodes.  It calls GetChildren() to iterate over a list of Sprites and sets their positions.  It also updates a Label's text once per frame.  This test aims to be a better emulation of real world api usage than the V1 tests.

| Language           | Bunnies Rendered |
|--------------------|------------------|
| GDScript (Release) | 12660            |
| C#/Mono            | 17236            |
| GDNative (D)       | 22620            |
| GDNative (Nim)     | 24315            |
| GDNative (C++)     | 31738            |

### BunnymarkV1 - DrawTexture

Attempts to draw as many sprites to the screen as possible by drawing textures directly with VisualServer.  This test focuses on compute / render performance and avoids making godot api calls.

| Language           | Bunnies Rendered |
|--------------------|------------------|
| GDScript (Release) | 14458            |
| C#/Mono            | 51718            |
| GDNative (Nim)     | 56123            |
| GDNative (D)       | 58060            |
| GDNative (C++)     | 60120            |

### BunnymarkV1 - Sprites

Attempts to draw as many sprites to the screen as possible by adding Sprite nodes.  This test focuses on compute / render performance and avoids making godot api calls.

| Language           | Bunnies Rendered |
|--------------------|------------------|
| GDScript (Release) | 12279            |
| C#/Mono            | 23279            |
| GDNative (Nim)     | 30479            |
| GDNative (D)       | 30636            |
| GDNative (C++)     | 34174            |

### Hardware:

* CPU: Intel i7 7700k 4.2GHz
* GPU: Nvidia GeForce GTX 1070
* RAM: 16GB DDR4

### Build Info:
* OS: Arch Linux
* Official Godot 3.0 release

## Credits

* GDScript example adapted from: https://github.com/curly-brace/godot-bunnies.  Thanks @curly-brace!
* @Capital-EX provided the initial Nim tests, the D tests, and the display server tests
* @endragor updated the GDNative tests to work with Godot 3.0 stable
