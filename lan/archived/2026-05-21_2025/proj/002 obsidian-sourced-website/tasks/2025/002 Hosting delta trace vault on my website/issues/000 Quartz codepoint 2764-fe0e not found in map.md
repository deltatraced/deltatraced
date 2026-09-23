# 1 Journal

* [ ] 

From [^spawn-issue-14d6dc](000%20Quartz%20codepoint%202764-fe0e%20not%20found%20in%20map.md#spawn-issue-14d6dc) in [3.1 Host Obsidian vault with Quartz](000%20Quartz%20codepoint%202764-fe0e%20not%20found%20in%20map.md#31-host-obsidian-vault-with-quartz)

````sh
npx quartz build
````

````
 ⠙ Emitting files: ContentPage -> public/lan/topics/practice/ctf/2025/google-ctf-2025/during/entries/000-overall.html

 ERROR 

 Failed to emit from plugin `CustomOgImages`: codepoint 2764-fe0e not found in map
     at loadEmoji (../util/emoji.ts:41:20)
     at Object.loadAdditionalAsset (../plugins/emitters/ogImage.tsx:58:22)
     at Array.map (<anonymous>)
     at Array.flatMap (<anonymous>)
     at generateSocialImage (../plugins/emitters/ogImage.tsx:52:15)
     at processOgImage (../plugins/emitters/ogImage.tsx:84:18)
     at Object.emit (../plugins/emitters/ogImage.tsx:120:9)
````

2025-09-02 Wk 36 Tue - 18:31

It's this note: *000 overall*

2025-09-02 Wk 36 Tue - 18:56

I opened an issue: [gh jackyzha0/quartz #2110](https://github.com/jackyzha0/quartz/issues/2110).

For now, removing the bad files during generation to move forward with this.
