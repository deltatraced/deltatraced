# What?

Here I document what I am currently up to.

# Also

* For my posts, see [002 Posts](lan/archived/2026-05-21_2026/main/wiki/001%20Wiki%20Categories/entries/002%20Posts.md).
* I have an inbox of tasks and investigations I work on at [002 Inbox](lan/2026/main/entry/002%20Inbox/002%20Inbox.md). See below for a more coarse description of what I'm up to.
* Devlog-like logs in [001 Weekly Act Log](lan/2026/main/entry/006%20Now%20Logs/entry/001%20Weekly%20Act%20Log.md).
* Thoughts and comments can be found in [007 Thought Stream Weeks](lan/2026/main/entry/007%20Thought%20Stream%20Weeks/007%20Thought%20Stream%20Weeks.md).
* Notes on this repository follow the clusterline method. To learn more about its underlying concepts, see [003 Wiki Clusterline Concepts](lan/2026/topic/concept/000%20Atomic/wiki/003%20Wiki%20Clusterline%20Concepts/003%20Wiki%20Clusterline%20Concepts.md).
* For a more general (but sorta outdated as-of-now oops) catalog, see [lan/archived/2026-05-21_2026/main/wiki/001 Wiki Categories/001 Wiki Categories](lan/archived/2026-05-21_2026/main/wiki/001%20Wiki%20Categories/001%20Wiki%20Categories.md).
* For more about now pages, see https://sive.rs/nowff.

# Sharing

I am looking for others who build public note taking systems like me for inspiration, discussion, collaboration, etc.

If you use a similar system or know of such a system, please share it in https://codeberg.org/deltatraced/deltatraced/issues/1.

# Current Focus

For prior focus, see [000 Was in focus](lan/2026/main/entry/006%20Now%20Logs/entry/000%20Was%20in%20focus.md).

2026-08-27 Wk 35 Thu - 01:59 +03:00

Getting back to studying [cubical type theory](https://arxiv.org/abs/1611.02108) using [agda](https://github.com/agda/agda) and [mikan](https://codeberg.org/1lab/mikan). I need to [review and refactor my solution for the CQTS lecture notes in mikan](lan/2026/topic/study-math/000%20CQTS%20Intro%20to%20Cubical/wikiproc/000%20Wiki%20Proc%20CQTS%20Intro%20to%20Cubical/task/000%20Reproduce%20how%20far%20we%20got%20in%20cqts%20but%20in%20problems-mkn%20module%20format.md). The lecture notes are [here](https://github.com/CQTS/introduction-to-cubical/).

Over at https://codeberg.org/lan22h-experiments I have been experimenting with multi-experiment-project repos with a precise indexing scheme.

For example, often when working with software we are interested in properties about our tools or examples of how to use them. When we use a programming language, specific errors can be vague and only after some effort we provide a clear explanation of why we get some obscure error. I am using https://codeberg.org/lan22h-experiments/code-examples as an index for these sorts of problems, where many projects therein are *claims* about the tool at hand, with the executable code serving to prove those claims. These often link back to specific notes I take here, which can provide more context about how I ran into these problems in the first place.

I also recently decided that notes here can be associated with files. Can be useful for things like my own solutions to tutorials, pastebins that would otherwise make the note too long, etc. This is indexed by the note hash, so that it is easy to go back and forth between here and the files: https://codeberg.org/lan22h-experiments/note-files

After I do some more work on it, I will also publish `problems-mkn` which, like [code-examples](https://codeberg.org/lan22h-experiments/code-examples), is a multi-project repository with a specified indexing scheme. But unlike it, the projects are all expected to be able to refer to one another from the git root and are written in [mikan](https://codeberg.org/1lab/mikan).

Mathematics is interconnected, so this can serve as a method to have many focused projects with only the dependencies required to solve the underlying problems. Having it as a graph of dependencies of many tiny projects can also help us replace some solution for another, which is useful for educational purposes.
This can also include common building block libraries that may mature out of `problems-mkn` into their own libraries.

**3D Printing**

A friend of mine wants me to print him a small PC case with my 3D printer, so I have been doing some research on using PETG filament for this. Since I am now on a gentoo system, I am reinstalling and configuring my modeling software.

**Gentoo Setup**

I recently installed Gentoo on my main PC: [001 Install a new Gentoo system](lan/2026/main/task/001%20Install%20a%20new%20Gentoo%20system/001%20Install%20a%20new%20Gentoo%20system.md), so currently configuring a lot of things as I go!

**Wanna get back to**

* Want to visualize cubes, and trying to do it with Agda, though currently trying to build cubical agda with the standard library
  * [lan/2026/microproj/000-labeled-cube-rendering/entry/000 Getting Started for LCR/task/000 Compile cubical agda to an executable using ctqs sources](lan/2026/microproj/000-labeled-cube-rendering/entry/000%20Getting%20Started%20for%20LCR/task/000%20Compile%20cubical%20agda%20to%20an%20executable%20using%20ctqs%20sources.md)

**Wanna look into**

* Setup mikan and investigate some good first issues
  * [lan/archived/2026-05-21_2026/topic/contribute/open source/possibly/cb 1lab mikan/issues/2026/000 mikan-88/000 mikan-88](lan/archived/2026-05-21_2026/topic/contribute/open%20source/possibly/cb%201lab%20mikan/issues/2026/000%20mikan-88/000%20mikan-88.md)

# Other project now pages

I also maintain different note repositories with a now page and inbox:

**Active**

* [Golden-sun Reverse Engineering now](https://github.com/FutureFractal/goldensun-notes/blob/webview/lan/now.md)
* [Megaman Battle Network 6 Reverse Engineering now](https://github.com/dism-exe/dism-exe-notes/blob/webview/lan/now.md)
