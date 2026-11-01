# 1 Journal

* [x] 

From [^spawn-task-781da8](008%20Handle%20loaded%20param16%20flag%20automatically%20in%20commands%20when%20reconstructing%20tape%20program.md#spawn-task-781da8) in [3.11 Reconstructing tape content](008%20Handle%20loaded%20param16%20flag%20automatically%20in%20commands%20when%20reconstructing%20tape%20program.md#311-reconstructing-tape-content)

The script for this is [here](https://github.com/LanHikari22/lan-exp-scripts/blob/main/files/2025/persistent/000-mountain-n-dragon-ctf/data/tape.py).

### 1.1.1 Passing None as a command byte issue

2025-08-01 Wk 31 Fri - 06:49

I'm trying to pass the command byte along with the content so that I check for the first bit. I decided to just follow a simple convention of `cmdlNN` for loaded commands and `cmdNN` for non-loaded. And I just have to mirror all the commands.

Right now it seems like I'm passing `None` for some reason for the byte to `format_command` :

````
[None, 512]

    if command_byte % 2 == 1:
       ~~~~~~~~~~~~~^~~
TypeError: unsupported operand type(s) for %: 'NoneType' and 'int'
````

This is from `scan_next_command`,

````py
content = (
	[command_byte] + command.param_widths
	| pipe.OfIter[int].enumerate()
	| pipe.OfIter[Tuple[int, int]].map(
		pipe.tup2_unpack(enumerated_param_width_to_read_value)
	)
	| pipe.OfIter[int].to_list()
)

print('content', content)

# out
# ...
content [None, 512]
content [None, 6154]
content [None, 778]
content [None, 1]
content [None, 13232]
````

2025-08-01 Wk 31 Fri - 06:58

This assert here is wrong... this is always true! I wanted to throw an error here.

````py
    def enumerated_param_width_to_read_value(i: int, width: int) -> int:
        if width == 16:
            return (tape[i + 2] << 8) + tape[i + 1]
        elif width == 8:
            return tape[i + 1]
        else:
            assert f"unsupported width {width}"
````

Changing it to `raise Exception(f"unsupported width {width}")` gives us

````
Exception: unsupported width 0
````

It's an order of operations thing...

````py
content = (
	[command_byte] + (command.param_widths
	| pipe.OfIter[int].enumerate()
	| pipe.OfIter[Tuple[int, int]].map(
		pipe.tup2_unpack(enumerated_param_width_to_read_value)
	)
	| pipe.OfIter[int].to_list())
)
````

Need to add `[command_byte]` to the rest, not add and then pipeline process!

2025-08-01 Wk 31 Fri - 07:05

Okay this works!

Now we use `cmdl` instead of `cmd` if the bit is set!
