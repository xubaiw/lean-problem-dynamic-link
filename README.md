# Problems with Lean Dynamic Link 

## How to reproduce the error

```sh
$ cd bar1
$ lake build
...
$ cd ../bar2
$ lake build
...
$ ./build/bin/bar2  -- running the program produce the error
dyld[66116]: Library not loaded: ./build/lib/libbar1.dylib
  Referenced from: /Users/xubaiw/bar/bar2/build/bin/bar2
  Reason: tried: './build/lib/libbar1.dylib' (no such file), '/usr/local/lib/libbar1.dylib' (no such file), '/usr/lib/libbar1.dylib' (no such file), '/Users/xubaiw/bar/bar2/build/lib/libbar1.dylib' (no such file), '/usr/local/lib/libbar1.dylib' (no such file), '/usr/lib/libbar1.dylib' (no such file)
zsh: abort      ./build/bin/bar2
```

## Details

In this repo we have two lean packages: `bar1` and `bar2`.

In `bar1` there is:

```lean
@[export bar1_greet]
def greet (name : String) := s!"Hello, {name}!"
```

Then `bar1` is compiled to  a sharedLib. In order to use the definition, in `bar2` we have:

```lean
@[extern "bar1_greet"]
constant greet : String → String

def main : IO Unit :=
  IO.println <| greet "Lean Programmers"
```

Both `moreLibTargets` and `moreLinkArgs` is used in `bar2`.

