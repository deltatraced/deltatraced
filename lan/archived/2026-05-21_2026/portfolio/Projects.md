Welcome! Let's walk through some of my work!

# 1 Projects

## 1.1 bn6f reverse engineering

Github: [bn6f](https://github.com/dism-exe/bn6f)
Languages: ARM Assembly, C, Python, Rust

Reverse engineering Mega Man Battle Network 6. This reproduces the original game data from source code. It also has many derivative projects for tooling and analysis

Those include:

* [Related IDA Analysis](https://github.com/LanHikari22/GBA-IDA-Pseudo-Terminal)
* [Automated generation of struct layouts using lua](https://github.com/LanHikari22/GBA_Memory-Access-Scanner)
* [Automated dumping of internal textscript within the game](https://github.com/LanHikari22/bn_textscript_dumper),

Upcoming:

* [high level editing of bn6f repository, sync with ghidra, and exporting type info for gdb](https://github.com/LanHikari22/bn_repo_editor)

To learn more, see [Project - bn6f](Project%20-%20bn6f.md).

## 1.2 dbmint

github: [dbmint](https://github.com/LanHikari22/dbmint)
Languages: Rust, SQLite

Creating databases from scratch can be very intuitive. I learned it was possible with  [dbml](https://dbdiagram.io/home) to create schemas very quickly in a markdown like language and also see them visualized as graph of connections.

I wanted to bring that intuition to sqlite3 database tooling. dbmint is able to take a dbml schema file and generate a sqlite3 database file. It supports docker, and so many operations that make dbml file use concrete can be done in one line, on any system. It also supports sync operations and more. See the [README](https://github.com/LanHikari22/dbmint) for more details.

Upcoming:

* Creating a language dbmt that extends dbml but allows multi file support, error validation, computed fields, and much more.
* Generating a database library in Rust for a given dbmt schema to give types for all columns and tables and prevent invalid query use via the compiler.

## 1.3 TTM

github: [TTM](https://github.com/LanHikari22/TTM)

Stands for Time & Task Management. It does what it means, and much more. What started as a project for being able to log time and open a web calender schedule all in the terminal grew to be able to handle a full graph-based note taking style. This project uses docker to ensure that a note server can be spawned as easily as possible. It integrates time, task, and notes together via vim and tmux and can even be used to extract analytics on the tasks over time.

To learn more, see [Project - TTM](Project%20-%20TTM.md).

## 1.4 checkpipe

github: [checkpipe](https://github.com/LanHikari22/checkpipe).
Languages: Python

This is a library I wrote to introduce data pipes with type hints into python, so that I can write python in a more functional way!

The project has many use case examples in the [README](https://github.com/LanHikari22/checkpipe/blob/main/README.md).

# 2 WIP Projects

## 2.1 Teensy2 Tiny Piano

github: [teensy2-tiny-piano](https://github.com/delta-domain-rnd/teensy2-tiny-piano)
Languages: C

This is a project where we use a keypad, toy magnet speakers, a self-built DAC with an RC circuit, and a teensy2 microcontroller to simulate a tiny piano!

The notes do play, however the quality of sound needs to be improved. This may require getting better parts.

# 3 Activities

## 3.1 IEEE Xtreme

![Pasted image 20250811133610.png](../../../../attachments/Pasted%20image%2020250811133610.png)
