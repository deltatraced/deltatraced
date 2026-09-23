The project can be found on github: [bn6f](https://github.com/dism-exe/bn6f)

# 1 Experience

The purpose of this project is to understand the [Mega Man Battle Network](https://capcom.fandom.com/wiki/Mega_Man_Battle_Network) games, the Game boy Advance hardware, and reverse engineering methodology and tooling in general.

In ways, this is my childhood project. I have begun hacking these games since I was in middle school. I learned some ARM assembly and some C++ back then to be able to understand how people make amazing mods with this game. I participated with the forums and reproduced some of their work.

Time went on, and I went to university to become a computer engineer. I learned formally about computer architecture, low-level C and ARM programming, and embedded systems. I learned that there are public total software reconstruction projects for Pokémon, and I wanted to do the same for the Mega Man Battle Network series. While there is amazing work by the community, there was no integrated project that builds into the exact ROM in the cartridge as there was for Pokémon.

Quickly I have learned that the scale of the project was huge, especially for a 1-2 person team. There were over 9000 functions to reverse engineer in this game alone, many proprietary data formats, and on top of that, it was hand written in assembly. I have learned a lot in university, and now I see the game's inner working through new eyes because of my training. I set out to build analysis tools to extract all the functions and data and ensure that we can build the exact ROM again. This has been largely a success. Many of the analysis tools can be found here:

* [Related IDA Analysis](https://github.com/LanHikari22/GBA-IDA-Pseudo-Terminal)
* [Automated generation of struct layouts using lua](https://github.com/LanHikari22/GBA_Memory-Access-Scanner)
* [Automated dumping of internal textscript within the game](https://github.com/LanHikari22/bn_textscript_dumper),

Once I've graduated university, I was busy with career and learning so many new things, it was hard to keep up with working on this project. Again, the sheer scale of it was also intimidating, it felt like a project that cannot end, unlike any lab assignment I've done in university, or even my senior design project, they all have had a well-defined scope.

# 2 Upcoming work

Over time, I've learned to accept that some projects can be long-term, and motivation in them may shift with time. I've started working on this again, however, my work now is more interconnected. I'm interested in functional programming and reproducibility of my work and knowledge. So I am making tools to make editing the repository as declarative as possible, integrate my reverse engineering work with modern reverse engineering tools like Ghidra, and import all the type knowledge into debugging The GBA game with gdb.

You can find the new tool for this here: [bn_repo_editor](https://github.com/LanHikari22/bn_repo_editor)

# 3 Related vision

It would be an understatement to say that this project had taught me a lot about what it means to document a software project, or take notes. Being more than a decade-long project, I could watch my evolution in note taking style and development with it.

This project, and [TTM](Project%20-%20TTM.md) are the predecessors to my note taken methodology and mission to make my software development process as transparent as possible.

# 4 Achievements

* Used IDA and heuristics to export most of the code and data and ensure that it builds to the sha1sum checksum of the file in the original game cartridge.
  -Project is able to compress and decompress assets and still achieve identical file checksum
  -Built a tool to extract over 42,000 text script assets
* Built a macro system for structs in assembly to document ROM and RAM structs in the game
* Discovered data structures automatically through static analysis and extracting runtime information from the game while it's running
* [bn_repo_editor](https://github.com/LanHikari22/bn_repo_editor) is able to process 11.4 million tokens across the entire repository in around 100 seconds.
