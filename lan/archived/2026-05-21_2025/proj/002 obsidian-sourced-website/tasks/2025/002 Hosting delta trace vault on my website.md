# 1 Objective

* [ ] Investigate various ways to get the obsidian vault up and running in www.deltatraced.com.
* [ ] Find hosting tech compatible with wasmer or host differently
* [ ] Post-process entries inside Tasks, Issues, HowTos, Investigations, Ideas, and Side Notes into their own pages
* [ ] Look into comment integration

# 2 Journal

# 3 Tasks

## 3.1 Host Obsidian vault with Quartz

* [ ] 

From [^spawn-task-a5c5eb](002%20Hosting%20delta%20trace%20vault%20on%20my%20website.md#spawn-task-a5c5eb) in [6.1 Look into available obsidian vault hosting solutions](002%20Hosting%20delta%20trace%20vault%20on%20my%20website.md#61-look-into-available-obsidian-vault-hosting-solutions)

2025-09-02 Wk 36 Tue - 18:04

Following [quartz Get Started](https://quartz.jzhao.xyz/),

````sh
git clone https://github.com/jackyzha0/quartz.git ~/src/cloned/gh/jackyzha0/quartz
cd ~/src/cloned/gh/jackyzha0/quartz
npm i
npx quartz create
````

2025-09-02 Wk 36 Tue - 18:18

It just asked to import the content, automatically understood it's an obsidian vault, and asked about the link setting to choose, and that was it!

Spawn [4.1 Quartz codepoint 2764-fe0e not found in map](002%20Hosting%20delta%20trace%20vault%20on%20my%20website.md#41-quartz-codepoint-2764-fe0e-not-found-in-map) ^spawn-issue-14d6dc

2025-09-02 Wk 36 Tue - 19:22

Gonna await on the issue and try other methods for now

### 3.1.1 Later

## 3.2 Host Obsidian vault with Perlite

* [ ] 

From [^spawn-task-5bd793](002%20Hosting%20delta%20trace%20vault%20on%20my%20website.md#spawn-task-5bd793) in [6.1 Look into available obsidian vault hosting solutions](002%20Hosting%20delta%20trace%20vault%20on%20my%20website.md#61-look-into-available-obsidian-vault-hosting-solutions)

2025-09-02 Wk 36 Tue - 19:30

[gh secure-77/Perlite](https://github.com/secure-77/Perlite).

Some settings to consider: [Perlite required-settings](https://github.com/secure-77/Perlite/wiki/03---Perlite-Settings#required-settings).

2025-09-02 Wk 36 Tue - 21:22

It mostly works. Still images and graph to fix.

Here is a wasmer deployment example: [php-wasmer-starter](https://github.com/wasmer-examples/php-wasmer-starter)

2025-09-02 Wk 36 Tue - 21:28

We can't seem to be able to deploy with `/perlite` as the name... This is from the wasmer deployment logs after `wasmer deploy`.

````
Directory /perlite does not exist.

Instance exited with code ExitCode::1
````

````sh
wasmer app delete
````

By changing the name to `app` it deploys, but...

![Pasted image 20250902213716.png](../../../../../../../attachments/Pasted%20image%2020250902213716.png)

````
[Tue Sep  2 18:32:50 2025] 127.0.0.100:8080 [404]: GET /.styles/katex.min.css - No such file or directory
[Tue Sep  2 18:32:50 2025] 127.0.0.100:8080 [404]: GET /.js/katex.min.js - No such file or directory
````

It can't find styles, or other files too like js.

2025-09-02 Wk 36 Tue - 21:41

When we move `wasmer.toml` and `app.yaml` to the app director, and change `app/` to `/`  we get

![Pasted image 20250902214211.png](../../../../../../../attachments/Pasted%20image%2020250902214211.png)

But it seems to require `/.styles`. It was at least able to load the content without the js and styles when we used `app/`. How come it can't retrieve anything in `/`?

````
[Tue Sep  2 18:41:29 2025] 127.0.0.100:8080 [404]: GET / - No such file or directory
````

And if we change the left hand value `/app`, nothing even gets logged, so we can't mess around with the filesystem bit there.

2025-09-02 Wk 36 Tue - 22:32

````
[Tue Sep  2 19:09:26 2025] 127.0.0.100:8080 [404]: GET /.js/perlite.js - No such file or directory
[Tue Sep  2 19:09:26 2025] 127.0.0.100:8080 [200]: GET /favicon.ico
````

These should be in the same place.

````sh
ls -al app          

# out (relevant)
-rw-rw-r-- 1 lan lan 15406 Sep  2 20:52 favicon.ico
drwxrwxr-x 2 lan lan  4096 Sep  2 20:52 .js
drwxrwxr-x 2 lan lan  4096 Sep  2 20:52 .scripts
drwxrwxr-x 2 lan lan  4096 Sep  2 20:52 .src
drwxrwxr-x 4 lan lan  4096 Sep  2 20:52 .styles
````

2025-09-02 Wk 36 Tue - 22:41

This is our current `wasmer.toml`:

````
[dependencies]
"php/php-32" = "=8.3.2101-rc.1"

[fs]
"/app" = "app/"

[[command]]
name = "run"
module = "php/php-32:php"
runner = "wasi"
[command.annotations.wasi]
main-args = ["-t", "app/", "-S", "localhost:8080"]
````

Let's try to find this ` =8.3.2101-rc.1` tag.

This tag isn't here: [wasmer-php](https://github.com/wasmerio/wasmer-php)

We found this package in the registry: [php/php-32](https://wasmer.io/php/php-32). and it is currently `8.3.2102`.

But where would that go? We don't know how the image is built.

2025-09-03 Wk 36 Wed - 11:27

So after changing the dot folders to not be dotted, it seems to work.

We have basic rendering:

![Pasted image 20250903112811.png](../../../../../../../attachments/Pasted%20image%2020250903112811.png)

That symbol for of-type fails to render under github LaTeX so it is nice that it is functional here.

### 3.2.1 Pend

# 4 Issues

## 4.1 Quartz codepoint 2764-fe0e not found in map

* [ ] 

### 4.1.1 Later

# 5 HowTos

# 6 Investigations

## 6.1 Look into available obsidian vault hosting solutions

* [ ] 

2025-09-02 Wk 36 Tue - 17:29

This [post](https://www.xda-developers.com/ways-host-obsidian-online/) suggests three free routes:

* [Obsidian Digital Garden](https://dg-docs.ole.dev/)
* [Quartz](https://quartz.jzhao.xyz/)
* [gh secure-77/Perlite](https://github.com/secure-77/Perlite)

They recommend to host in Github Pages with Quartz. That can still be something to explore, even though we have wasmer.

2025-09-02 Wk 36 Tue - 18:03

Let's try with quartz.

Spawn [002 Hosting delta trace vault on my website > 3.1 Host Obsidian vault with Quartz](002%20Hosting%20delta%20trace%20vault%20on%20my%20website.md#31-host-obsidian-vault-with-quartz) ^spawn-task-a5c5eb

2025-09-02 Wk 36 Tue - 19:22

There are build issues. I filed an issue [gh jackyzha0/quartz #2110](https://github.com/jackyzha0/quartz/issues/2110). let's try Perlite next.

Spawn [3.2 Host Obsidian vault with Perlite](002%20Hosting%20delta%20trace%20vault%20on%20my%20website.md#32-host-obsidian-vault-with-perlite) ^spawn-task-5bd793

### 6.1.1 Pend

# 7 Ideas

# 8 Side Notes

# 9 External Links

# 10 References
