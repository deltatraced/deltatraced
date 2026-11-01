Wiki Proc: [000 Wiki Proc Error Source Records](../../wikiproc/000%20Wiki%20Proc%20Error%20Source%20Records/000%20Wiki%20Proc%20Error%20Source%20Records.md)

# Purpose

The purpose of this is to build an index by language and/or by project and of errors encountered and then solved.

This should not show lengthy investigations and dead ends, but rather only a description of the issue, and the cause, followed by links to additional notes that dive further into how the error was encountered. This helps separate the records from the concerns of the context that encountered them, and allows for the same error to possibly be encountered in multiple contexts and be accumulated here.

# Format

In addition, the format of each record should be uniform for automatic parsing.

Linking beginning from this page for projects will go as follows: `forge -> user -> project -> error`. forge is like github, codeberg, etc. Language specific errors go into `lang > name > error`.

The `error` file's content interpretation depends on the header.

* `# Message` simply gives the error message. In case multiple contexts report the same message with some differences, just add `## Message N` to have many.
* `# Versions` Includes a description of the versions in use. Like `## Message N`, use `## Versions N` to associate multiple contexts.
* `# Example` gives an example that ought reproduce the error.
* `# Description` gives a broad description of the error,
* `# Reproduction` gives instructions to reproduce it from a fresh setup,
* `# Solution`  points to an action that reproduces and resolves the issue and a brief causal explanation.
* `# Cause` is for the root cause of the problem. Omission to include this header is vague to interpret. If it were investigated, but not found, then it should be marked `Unknown`, if it was not investigated, write in the header `Not Investigated`. An investigation note may also be linked for further context at the end in bullet points.
* `# Resources` has in bullet points related sources. A git repository path is expected to mean a reproduction in code.
* `# Related` any related notes that encounterd the issue go here in bullet points with a subpoint optionally for a description of the encounter.

To ensure unique filenames, all filenames are prefixed with `NNN WESR`, standing for `Wiki Error Source Records`, and every `error` starts with `NNN WESR {proj-name}`.

# Index

* [000 WESR Github](entry/000%20WESR%20Github.md)
