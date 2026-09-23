# Idea

The idea is to only ever have the choice to switch between two alternatives, and instead up the degrees of freedom of how this switching happens. With only one degree of freedom, we can switch between two contexts. For example, you have a window with exactly two tabs, and you alternate between them.

A cube-2 ups this to 4:

````
ctx10 <-> ctx11
  ^         ^
  |         |
  v         v
ctx00 <-> ctx01
````

In any context, you have now two modes of alternating: You can alternate right/left and up/down. Your system can differentiate 4 possibilities. You can attach meaning to what the "right" of a window is, and what the "down" of it is.

A cube-3 gives us even more leverage, with 8 possibilities:

````
        ctx110 <-----> ctx111
      /   ^           /  ^
     /    |          /   |
    /     |         /    |
ctx010 <-----> ctx011    |
  ^       |      ^       |
  |       |      |       |
  |       v      |       v
  |     ctx100 <-|---> ctx101
  |   /          |   /
  |  /           |  /
  v /            v /
ctx000 <-----> ctx001
````

So three distinct meanings of switching from any node. You can keep going if you want to cube-4 with 16 possibilities. This scales exponentially in that regard.

This is also to be used within a single *session*. Each session has its own cube, and a name. You use label fuzzy search to switch between sessions.
