---
status: done
---

# Objective

````json
[
	{ ... },
	{ ... },
	{ ... },
	{ broken...
````

Something like this should be turned into

````json
[
	{ ... },
	{ ... },
	{ ... }
]
````

Resolves when we have a bash cli function we can just to partial-fix json.

# Resolution

The solution is in https://codeberg.org/lan22h-experiments/cli-tools/src/branch/main/topic/json/pr000-fix-partial-json-arr/src/main.sh

We are able to use a grep-based solution for the last complete layer 1 object.

OK

# Journal

2026-08-31 Wk 36 Mon - 05:18 +03:00

Generate some JSON with broken output,

````sh
su
tshark -ieno1 -yDOCSIS -Tjson | tee /home/lan/t/ta
````

````sh
function fix-partial-json-arr() {
	inp="$(cat /dev/stdin)"
	len=$(echo "$inp" | wc -l)
	
	for ((i=0;i<$len;i++)) do
		partial="$(echo -e "$(echo "$inp" | head -n "$(expr $len - $i)" | head -c-2 )"\\n])"
		json="$(echo "$partial" | jq 2>/dev/null)"
		valid=$?
		[ $valid -eq 0 ] && echo "$json" && return 0
	done
	
	return 1
}
````

So we can now just do `cat ~/t/ta | fix-partial-json-arr | jq | less`

2026-08-31 Wk 36 Mon - 06:23 +03:00

This works but it looks overkill to be testing over every line when we know there is a deterministic fix for this.

We can get the last line of the expected tab with `cat ~/t/ta | grep -n '^  },' | tail -n1 | cut -d':' -f1`

````sh
function fix-partial-json-arr() {
	inp="$(cat /dev/stdin)"
	last_layer_1_obj_lineno="$(echo "$inp" | grep -n '^  },' | tail -n1 | cut -d':' -f1)"
	partial="$(echo -e "$(echo "$inp" | head -n$last_layer_1_obj_lineno | head -c-2)"\\n])"
	
	echo "$partial"
}
````

2026-08-31 Wk 36 Mon - 06:43 +03:00

````
# in /home/lan/src/cloned/cb/lan22h-experiments/cli-tools
git commit # out { [main faf7014] json/pr000: Add partial JSON fix script }
````

2026-08-31 Wk 36 Mon - 07:18 +03:00

Seems it's not always working right. Over a different wireshark capture I get

````
jq: parse error: Invalid string: control characters from U+0000 through U+001F must be escaped at line 967, column 1
````

````
"http.request.line": "MAN: \"ssdp:discover\"\r\n",
````

Ouch I interpreted this with `echo -e`!

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/cli-tools/topic/json/pr000-fix-partial-json-arr/src/main.sh
#!/bin/sh

function fix-partial-json-arr() {
	inp="$(cat /dev/stdin)"
	last_layer_1_obj_lineno="$(echo "$inp" | grep -n '^  },' | tail -n1 | cut -d':' -f1)"
    partial="$(echo "$inp" | head -n$last_layer_1_obj_lineno | head -c-2)"
	
	echo -e "${partial}\\n]"
}
````

This still reproduces the problem, that `-e` is interpreting into `${partial}`.

We can concat without the `-e`:

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/cli-tools/topic/json/pr000-fix-partial-json-arr/src/main.sh
#!/bin/sh

function fix-partial-json-arr() {
	inp="$(cat /dev/stdin)"
	last_layer_1_obj_lineno="$(echo "$inp" | grep -n '^  },' | tail -n1 | cut -d':' -f1)"
    partial="$(echo "$inp" | head -n$last_layer_1_obj_lineno | head -c-2)"
    completion="$(echo -e \\n])"
	
	echo "${partial}${completion}"
}
````

This fixes the issue.

````sh
# in /home/lan/src/cloned/cb/lan22h-experiments/cli-tools
git commit # out { [main 5901003] json/pr000: Fix escapes being interpreted in json input }
````

OK

\**
