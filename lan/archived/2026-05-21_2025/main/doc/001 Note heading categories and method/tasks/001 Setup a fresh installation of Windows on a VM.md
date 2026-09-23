# 1 Journal

from [^spawn-task-603822](001%20Setup%20a%20fresh%20installation%20of%20Windows%20on%20a%20VM.md#spawn-task-603822) in [6.1 Investigate the Windows Operating System Internals](001%20Setup%20a%20fresh%20installation%20of%20Windows%20on%20a%20VM.md#61-investigate-the-windows-operating-system-internals)

2025-08-12 Wk 33 Tue - 13:45

We're back in the Tasks section from the Investigation section! Because this is a clearly operative thing to get done!

Now to simulate some work, let's go through a mini tutorial!

### 3.2.1 Time logging mini tutorial

You can learn more about setting this up in [obsidian: 000 Setting up time logging in Obsidian](https://github.com/LanHikari22/lan-setup-notes/blob/webview/lan/topics/tooling/obsidian/entries/2025/000%20Setting%20up%20time%20logging%20in%20Obsidian.md) from my other vault [lan-setup-notes](https://github.com/LanHikari22/lan-setup-notes/tree/webview).

We can also time log against any heading in Obsidian using my time logging scripts.

Just put your cursor at the heading, like this

\<![Pasted image 20250812145428.png](../../../../../../../attachments/Pasted%20image%2020250812145428.png)

and then do `Alt+k Alt+e`

Given that you've configured this plugin with your user, you see

\<![Pasted image 20250812144507.png](../../../../../../../attachments/Pasted%20image%2020250812144507.png)

Write "Sta"  and select "{user} Start Log"

\<![Pasted image 20250812144536.png](../../../../../../../attachments/Pasted%20image%2020250812144536.png)

Now let's do Open Timeline Log to check out what happened!

\<![Pasted image 20250812144657.png](../../../../../../../attachments/Pasted%20image%2020250812144657.png)

Notice you can click the heading you started this with or even hover over it to see its content:

\<![Pasted image 20250812144747.png](../../../../../../../attachments/Pasted%20image%2020250812144747.png)

When we're done, we can do `Alt+k Alt+e Stop` or just stop from the logger UI directly.

\<![Pasted image 20250812144855.png](../../../../../../../attachments/Pasted%20image%2020250812144855.png)

We can do `Alt+k Alt+e Summarize Time Logs` and this will create a summary by week and all time over in the same director as the timeline.

\<![Pasted image 20250812145037.png](../../../../../../../attachments/Pasted%20image%2020250812145037.png)

This explained how we can get timing information for our notes, and specifically where!

If you're curious about the WkNN, those are week numbers according to the [ISO week date](https://www.epochconverter.com/weeks/2025>>>>>>>).

OK Let's go back to our [spawner](001%20Setup%20a%20fresh%20installation%20of%20Windows%20on%20a%20VM.md#spawn-task-603822)
