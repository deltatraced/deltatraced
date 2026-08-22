  
# Journal

---

2026-05-21 Wk 21 Thu - 14:31 +03:00

Here is a new way of writing journal entries.

-- 2026-05-21 Wk 21 Thu - 14:33 +03:00
They can come at levels to mark that they are at the next level. The text though remains document-like.
    
---- 2026-05-21 Wk 21 Thu - 14:34 +03:00
This is even more indented. 

------  2026-05-21 Wk 21 Thu - 14:41 +03:00
You get the idea.


---

2026-05-21 Wk 21 Thu - 14:41 +03:00

Part of the restructering is to consolidate the repository knowledge system rules. The prior version `archive-2025-may-21` was very permissive with folder structure. 

Here is how it will be now

/user
  /.archive
    yyyy-mm-dd{-pre}
    archive-reason # Keeps logs for each archive
  /YYYY
    /proj       # Main project notes go here
    /microproj  # Smaller projects go here
    /post       # Post documents
    /wiki       # Central wiki clusters go here.
      NNN-{cluster-name}
    /wikiproc
      NNN-proc-{cluster-name} # Every wiki cluster has a dual proc
    /task       # Central tasks go here
    /entry      # Central entries go here
    /doc       # Document clusters and/or files go here.
    /file      # More misc files, may include logs.
    /topic      # These are now flat projects. Use tags and links.

The `YYYY` project is special, in that it includes things like `/post`, `/topic`, `/proj`, etc. 

Other projects can be created under `/proj`, `/microproj`, and `/topic`. 

A project typically has the following structure:

/NNN-my-proj
  /entry
  /task
  /issue
  /wiki
  /wikiproc
  /doc
  /file

Inside `/task`, `/entry`, `/wiki`, and `/wikiproc` are clusters. Clusters are like a middle layer: they organize notes into a common purpose. A cluster is recognized by having a file by its own name: `NNN-my-cluster/NNN-my-cluster`, called its core note. It then allows spawning subnotes of different context types, and will put them in a flat per-category folders underneath it. 

Clusters can be put in status folders: `/todo, /done, /wtch, /idea, /also`

---

2026-05-21 Wk 21 Thu - 15:03 +03:00

We also want to rebrand away from Obsidian. There are issues rendering its own flavor of markdown, and we don’t have much control over it. We had a `lan-obsidian-plugin` over there to automate creating clusters. Wherever we go, we need a plugin to automate the tasks of creating projects and clusters. We need to be able to quickly index projects and clusters. 

As you can see, besides the central categories, all projects are flat. And they are themselves containing flat clusters. Nesting should be limited. Before, we did not have wikis, and we relied heavily on the folder structure to navigate. But we should not have to. We should simply move to the project of interest and/or the cluster of interest, and creating them should all be automated.

Our first shot is to do this in Silverbullet.md. Unlike obsidian, it’s open source. It also uses CommonMarkdown, and so rendering should be easier to deal with, and we have lesser vendor lock in.

We’re still using obsidian a bit, because we don’t have a SilverBullet.md plugin yet to do the functions we do there, like spawning, but otherwise writing can be done in Silverbullet.md with no problem.

Let’s bring back some compatible items from the 2026-05-21 archive. In order to see the true state of the archive (as will be mentioned in the log), one will need to go to a specific commit. Otherwise we want to avoid needless duplication, so we will move what we need back.

One of the many images I https://github.com/epwalsh/obsidian.nvim#system-requirementshave:

![[attachments/Pasted image 20250629022907.png]]
In Silverbullet.md I had to add `attachments/`. It seems the default place for images changes here. Might have to do something about this, until they allow it to be configurable.

Also why can’t I do gg and G in vim here?

---

2026-05-21 Wk 21 Thu - 18:08 +03:00

Spawn [[000 Some test entry for 2026 may restructure]] ^spawn-entry-d41d18

Okay. Updated the obsidian plugin also to reflect the new folder names. `entry` instead of `entries`, etc.

Of course this just doesn’t work automatically for us with Silverbullet. The links are short, and it expects a full path.

[[000 Some test entry for 2026 may restructure]]


2026-06-01 Wk 23 Mon - 13:02 +03:00

Some other open source note taking tools to consider:
- https://github.com/siyuan-note/siyuan
- https://github.com/laurent22/joplin
- https://github.com/epwalsh/obsidian.nvim
- https://github.com/helix-editor/helix
