---
context_type: task
status: done
---

Parent: [lan/2026/topic/tutorials/000 Tutorials/entry/000 idris-community idris2-tutorial/000 idris-community idris2-tutorial](../000%20idris-community%20idris2-tutorial.md)

Spawned by: [lan/2026/topic/tutorials/000 Tutorials/entry/000 idris-community idris2-tutorial/000 idris-community idris2-tutorial](../000%20idris-community%20idris2-tutorial.md)

Spawned in: [^spawn-task-6928b0](../000%20idris-community%20idris2-tutorial.md#spawn-task-6928b0)

# Journal

2026-09-05 Wk 36 Sat - 20:13 +03:00

[gh idris2-lsp](https://github.com/idris-community/idris2-lsp) recommends using `idris2 --init` to start a new project.

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut
idris2 --init

# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut/tut.ipkg {
	package tut
	sourcedir = "src"
# }
````

https://github.com/idris-community/idris2-lsp/blob/main/idris2-lsp.ipkg could be good reference.

https://github.com/idris-community/idris2-lsp/tree/main/src/Language/LSP for non-literate idris2, the files are `*.idr`.

In a bunch of projects in Idrs-community, the convention is capitalized file names and folders for `*.idr` files.

[gh idris2-tut](https://github.com/idris-community/idris2-tutorial)

2026-09-05 Wk 36 Sat - 20:57 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut/tut.ipkg {
	executable = tut
# }
````

We need to set this or we get an error from `pack run`:

````
[ fatal ] Package /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut/tut.ipkg is not an application
````

https://idris2.readthedocs.io/en/latest/tutorial/packages.html

2026-09-05 Wk 36 Sat - 21:09 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut/tut.ipkg {
	package tut
	main = Main
	executable = tut
	sourcedir = "src"
# }

# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut/src/Main.idr {
	main : IO ()
	main = putStrLn "Hello World!"
# }

# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut
pack run

# out
Hello World!
````

Though we're having issues with `src/App/Main.idr` instead.

Let's try to organize it like https://github.com/idris-community/katla

2026-09-05 Wk 36 Sat - 21:46 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut
tree .

# out
.
├── build
│   ├── exec
│   │   ├── tut
│   │   └── tut_app
│   │       ├── compileChez
│   │       ├── libidris2_support.so
│   │       ├── tut.so
│   │       └── tut.ss
│   └── ttc
│       └── 2025081600
│           ├── App
│           │   ├── Comm.ttc
│           │   └── Comm.ttm
│           ├── App.ttc
│           └── App.ttm
├── README.md
├── src
│   ├── App
│   │   └── Comm.idr
│   └── App.idr
└── tut.ipkg
````

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut/tut.ipkg {
	package tut
	modules = App
	        , App.Comm
	main = App
	executable = tut
	sourcedir = "src"
# }

# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut/src/App.idr {
	module App
	
	import App.Comm
	
	main : IO ()
	main = hello
# }

# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut/src/App/Comm.idr {
	module App.Comm
	
	export
	hello : IO ()
	hello = putStrLn "Hello World!"
# }

# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut
pack build

# out
[ info ] Using package collection nightly-260903
[ info ] Building: tut
[ build ] Now compiling the executable: tut

# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/proj/9e3cf2l-idris2-community-tut/tut
pack run

# out
Hello World!
````

This makes a project with an executable, specifies main, and demonstrates a minimal exported function across modules.

2026-09-07 Wk 37 Mon - 09:18 +03:00

https://github.com/stefan-hoeck/idris2-pack recommends to use `pack new lib {libname}` also.

The main difference with this approach is the `pack.toml`, and an additional `test` inner project:

````sh
# in pack.toml {
	[custom.all.tut]
	type = "local"
	path = "."
	ipkg = "tut.ipkg"
	test = "test/test.ipkg"
	
	[custom.all.tut-test]
	type = "local"
	path = "test"
	ipkg = "test.ipkg"
# }
````

````sh
# output of tree
.
├── pack.toml
├── src
│   └── Tut.idr
├── test
│   ├── src
│   │   └── Main.idr
│   └── test.ipkg
└── tut.ipkg
````

We can also new `pack new bin {libname}`

Rewrote our example app in terms of this.

2026-09-07 Wk 37 Mon - 09:33 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/note-files/
git commit # out { [main 8ef1dee] 9e3cf2: hello world proj for idris 2 }
````

OK

# References

1. [gh idris2-lsp](https://github.com/idris-community/idris2-lsp)
1. [gh idris2-nvim](https://github.com/idris-community/idris2-nvim)
1. [gh idris2-tut](https://github.com/idris-community/idris2-tutorial)
1. [idris2-tut](https://idris-community.github.io/idris2-tutorial/Tutorial/Intro.html)
