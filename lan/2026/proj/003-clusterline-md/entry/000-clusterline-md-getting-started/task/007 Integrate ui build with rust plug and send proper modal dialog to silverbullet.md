---
context_type: task
status: done
---

Parent: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/000-clusterline-md-getting-started](../000-clusterline-md-getting-started.md)

Spawned by: [lan/2026/proj/003-clusterline-md/entry/000-clusterline-md-getting-started/task/005 Add a custom fuzzy selector window with some text](005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md)

Spawned in: [^spawn-task-8017b4](005%20Add%20a%20custom%20fuzzy%20selector%20window%20with%20some%20text.md#spawn-task-8017b4)

Overview: [000 Overview clusterline getting started](../entry/000%20Overview%20clusterline%20getting%20started.md)

# Journal

2026-07-04 Wk 27 Sat - 15:37 +03:00

````
- [ ] Integrate ui build with rust plug and send proper modal dialog to silverbullet
````

So we want to build `clusterline-ui`, and copy the necessary data over to `clusterline-rs`'s `./src/inc` folder. We can't just use `./inc` because

````rust
let x = &format!("{}", include!("./inc/a00"));
````

Fails by telling us it looked under `src/./inc/a00`.

````rust
let x = &format!("{}", include!("./inc/a00.rs"));
````

It seems though this only expects rust files. We have `include_str!`.

With `cargo expand | less` we see:

````rust
let x = &::alloc::__export::must_use({
	::alloc::fmt::format(format_args!("{0}", "This file has some test text!\n"))
````

for

````rust
let x = &format!("{}", include_str!("./inc/a00"));
````

We can just use

````rust
let x = include_str!("./inc/a00");
````

which yields via `cargo expand | less`

````rust
let x = "This file has some test text!\n";
````

2026-07-04 Wk 27 Sat - 18:10 +03:00

From `public/index.html` we need to either remove or inline `<script src="/index.js"></script>` since we will pass it to silverbullet. We can just remove it from the built `index.html` ourselves. Though it's best not to remove it manually so that we can still use `npm run watch` for local testing of widgets.

There is also `<link rel="stylesheet" , type="text/css" href="/index.css">` that we need to inline.

[000 Shell Cat Nth Line](../../../../../main/wikiproc/001%20Wiki%20Proc%20HowTos/howto/000%20Shell%20Cat%20Nth%20Line.md)

2026-07-04 Wk 27 Sat - 19:11 +03:00

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/ping-pong-score-ts/build.sh > commit ea90ec9

# Merge index.html and the javascript into a single file: build/index.html.

js_content="$(cat $script_dir/build/main.js)" && \
awk -v fix="$js_content" '/<!SCRIPT_HERE!>/ {$0 = fix}1' $script_dir/index.html > $script_dir/build/index.html && \
````

````sh
awk --help

# out (relevant)
-v var=value     assigns value to program variable var.
--               unambiguous end of options.

If {action} is omitted it is implicitly { print }.  If pattern is omitted, then it is implicitly matched. 

Once an input stream is open, each input record is tested against each pattern, and if it matches, the associated action is executed. 

A BEGIN pattern matches before any input has been read, and an END pattern matches after all input has been read.

gsub(r,s,t)  gsub(r,s)
                   Global substitution, every match of regular expression r in variable t is replaced by string s.  The number of replacements is returned.  If t is omitted, $0 is used.  An
                   & in the replacement string s is replaced by the matched substring of t.  \& and \\ put  literal & and \, respectively, in the replacement string.
````

````sh
echo "AAA BBB CCC\nCCC DDD EEE" | awk '/DDD/ {print $1; print $2; print $3}'

# out
CCC
DDD
EEE
````

````sh
echo "AAA BBB CCC\nCCC DDD EEE" | awk -v myvar="beep" 'END {print myvar}'

# out
beep
````

````sh
echo "AAA BBB CCC\nCCC DDD EEE" | awk -v myvar="beep" 'END {$2 = myvar; print}'

# out
CCC beep EEE
````

So we can see the above `awk` command only replaces an entire line.

Let's adapt this to integrate the generate css into the html file.

https://stackoverflow.com/a/3422309/6944447

````sh
echo "AAA BBB CCC\nCCC DDD EEE" | awk -v myvar="beep" '{gsub("CCC", "WEE", $0); print}'

# out
AAA BBB WEE
WEE DDD EEE
````

So to inline the styles our command becomes:

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterlinemd/clusterline-ui/build.sh

script_dir=$(dirname "$(readlink -f "$0")")
root_dir="$script_dir/.."

cd $root_dir/clusterline-ui && \
css_content="$(sed '1q;d' $root_dir/clusterline-ui/public/index.css)" && \
awk -v css_content="$css_content" '{gsub("<link rel=\"stylesheet\" , type=\"text/css\" href=\"/index.css\">", "<style>" css_content "</style>", $0); print}' $root_dir/clusterline-ui/public/index.html > $root_dir/clusterline-rs/src/inc/index.html
````

Now we extend it to also inline `<script src="/index.js"></script>`:

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterlinemd/clusterline-ui/build.sh

script_dir=$(dirname "$(readlink -f "$0")")
root_dir="$script_dir/.."

cd $root_dir/clusterline-ui && \
tmp1=$(mktemp) && \
css_content="$(sed '1q;d' $root_dir/clusterline-ui/public/index.css)" && \
awk -v css_content="$css_content" '{gsub("<link rel=\"stylesheet\" , type=\"text/css\" href=\"/index.css\">", "<style>" css_content "</style>", $0); print}' $root_dir/clusterline-ui/public/index.html > $tmp1 && \
js_content="$(cat $root_dir/clusterline-ui/public/index.js)" && \
awk -v js_content="$js_content" '{gsub("<script src=\"/index.js\"></script>", "<script>" js_content "</script>", $0); print}' $tmp1 > $root_dir/clusterline-rs/src/inc/index.html && \
rm $tmp1
````

This is confirmed working with styles and js behavior by opening `$root_dir/clusterline-rs/src/inc/index.html` in browser.

2026-07-04 Wk 27 Sat - 23:42 +03:00

We're able to put a test `<p>Hi</p>` and it appears through the build pipeline in silverbullet once passed to a panel. But the dialog is not there, which is invisible by default by enabled by an effect in the js script. This indicates that inlining the script is unexpected behavior, and that it should be passed separately from the html. Let's revise our UI component build. It's a small change, just replace the script inclusion with `""` and copy it for rust to use directly:

````sh
# in /home/lan/src/cloned/cb/lan22h/clusterlinemd/clusterline-ui/build.sh

#!/bin/sh

script_dir=$(dirname "$(readlink -f "$0")")
root_dir="$script_dir/.."

cd $root_dir/clusterline-ui && \
npm run build && \
echo "> inlining html" && \
tmp1=$(mktemp) && \
css_content="$(sed '1q;d' $root_dir/clusterline-ui/public/index.css)" && \
awk -v css_content="$css_content" '{gsub("<link rel=\"stylesheet\" , type=\"text/css\" href=\"/index.css\">", "<style>" css_content "</style>", $0); print}' $root_dir/clusterline-ui/public/index.html > $tmp1 && \
js_content="$(cat $root_dir/clusterline-ui/public/index.js)" && \
awk -v js_content="$js_content" '{gsub("<script src=\"/index.js\"></script>", "", $0); print}' $tmp1 > $root_dir/clusterline-rs/src/inc/index.html && \
cp $root_dir/clusterline-ui/public/index.js $root_dir/clusterline-rs/src/inc/ && \
rm $tmp1
````

````rust
// in /home/lan/src/cloned/cb/lan22h/clusterlinemd/clusterline-rs/src/lib.rs
let html = include_str!("./inc/index.html");
let script = include_str!("./inc/index.js");

editor::show_panel(PanelLocation::Modal, /*mode*/ inv(util::basic_types::U32Nz::new(1)), html, script).await;
````

Now it works, including with behavior and style, directly through silverbullet!

OK
