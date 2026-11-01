# 1 Abstract

Here we are going to contrast two styles of note taking that we call historical (non-editable) and document (editable). We discuss types of content that best fit a given style, and how those two styles can work together.

# 2 Pros and Cons of Historical Notes

Previously in posts [Note Taking Philosophy](../../2025/post/000%20Note%20Taking%20Philosophy.md) and [Atomic Contexts and Respect](../../2025/post/002%20Atomic%20Contexts%20and%20Respect.md), we described a way of writing notes that is composed as a series of "next considerations". We pick up a project, and we ask what's the next thing to do, this guides us in that we focus on the next task, the next problem, and so on. The goal of process notes is to capture at any moment, what we consider to be the current most important thing, or the current insight or idea we have.

This ends up looking like a stream, always flowing down with new content. This can significantly decrease the frustration of figuring out where to capture ideas. For a stream, it is simple. Always at the end.

Because of this linear additive fashion of "what's next", these notes accumulate a history of our thought or task activity. They can also give us insight into the process we've undertaken, because we can look back at the trail of linear notes like historical line of how a specific idea developed. If the list captures key insights of how a work was created, this can also help us make that creation process more reproducible by us and others.

This approach has some restrictions. It favors immutable traces of notes. This means that as we enter a new note to the list, we should not be modifying the previous ones so that they can function as an auditable trail and a reliable history for what happened.

But not all notes should function as a history. For example, if we're doing research, we want to be able to have a document or a paper that showcases our research. These notes can only emulate this, by serializing the document into chunks written in a list, which leads to a lot of duplication, but also confusion as it is now fragmented, and to make it less fragmented, we end up duplicating more to edit.

As another example, when problem solving, it can be desirable to capture not only a solution but an argument that guides us from initial considerations to a final acceptable solution to the problem. This has pedagogical value, and it also captures how we reason and solve problems. This is best treated as its own deliverable or document. We need to have the ability to proof read, edit, and refine this document with time.

One must note that with historical (/process) notes, the goal is not to capture the entirety of the creativity/discovery process, or even most of it. What gets left out is as important as what is included. Fewer but stronger insights to guide a future reader to rediscover the creativity process is what is important, which can be done not by exhaustive description of what we are doing, but by journaling our encounters with the most important insights that lead to the creation of a work or learning something new.

# 3 Contrasting with Document Notes

Editable document notes lift the restriction (or tendency) to serialize notes on a timeline. This means they are free to take a different shape, such as a specific heading structure. Note that just like historical notes, these documents can also link to other documents, which are themselves editable. This means we have an entire network of notes that is malleable and can be edited and relinked. In contrast, networks of historical notes pin down any note they link for content.

By being editable, documents encourage meaningful deletions while historical notes restrict them or treats them with caution.

More on linking notes, we have noted in [Atomic Contexts and Respect](../../2025/post/002%20Atomic%20Contexts%20and%20Respect.md) that should make good use of linking and creating new notes. The name of a note denotes a given context we are operating in. This allows us to scope our activity and stay focused, giving full attention to critical parts without being distracted by any unnecessary details. Those details can be put away in a different context or file, and linked to, reducing a lot of noise down to a footnote.

Editable notes also help us here. In allowing a more liberal use of deletion, we can significantly cut down on noise in the writing process, since we no longer need to be as concerned with preserving the content of the document, and we're no longer restricted to only editing in new content. We can continue to make use of the previous scoping principles as well with editable notes.

We noted before that documenting arguments is best done as a document rather than just history. In general we call these explanations. An explanation is meant to be communicated to us in the future and to others, and thus should be heavily refined, and any unnecessary content should be cut down. If we do not do this, we would be wasting our time and others who read through it in the future in an unrefined form with much duplication.

Explanations are also an interesting example because they also express a process, just like the historical notes. They take the reader from a starting point, and guide them towards a conclusion. For a programming example, we have literate programming that mixes source code in with text documents like markdown. This allows us not only to write source code, but write it incrementally, while expressing our varying attention and use of resources at our disposal. Tools like Jupyterlab notebooks are valued for similar reasons, mixing a linear document with code, and also running that code, which may generate different graphs or tables of interest.

The process notes we have, on the other hand, act as a data-gathering instrument. They do not directly explain. Rather, they capture ideas at different points in time, and by examining this data, we may infer an explanation. Now we have a way to directly report this explanation in a document.

# 4 Synergy

So we talked about process/historical notes, and documents, but how do the two work together?

From [Apprenticeship to Signs](../../2025/post/003%20Apprenticeship%20to%20Signs.md), we have expressed a need to separate evidence from explanations. Our historical notes can act as a trail of evidence to be crystallized in explanation documents. However, there is a dual aspect to this. The process/historical side of forming a judgment is tasked with showing us how and where the evidence is that we will use. It makes heavy use of linking to other notes. This is all important work, but can be considered secondary to the resulting explanation itself. Often in papers, much work is done researching, but the report makes use of the links rather than explicates the researching day-to-day activity.

By having a mirror process note for every document note, the process note can capture the auditable trail of *researching* and linking things and justifying that, while the document end expresses the final refined use of resources at our disposal to reach a conclusion.

We have also already been operating with both a document and a process note at hand here. Often source code acts as the deliverable, and much experimentation goes there. When some important progress is encountered, it is then captured in the process notes. In other words, having a document allowed us to distill the process notes to capture only the important encounters. We can make use of this same advantage when we have a mirror process note for a document note. By making good use of deletion and allowing the shape of the document to guide our thinking and creating, we can capture fewer but more insightful historical notes of progress.

The dual use of process note and document note also opens a new dimension of critique and collaboration with others. The process side can be examined for patterns of thought and attention that others may examine for information that is hard to find in the document itself. The document of course can be much more easily shared with others, and meet shared quality standards. Readers can focus on the document to assess the quality of content, and on the process note to assess the choice of methods used and how and when they are used.

# 5 Challenges

A strong benefit of process notes is that their immutability makes document versioning basically trivial. This is no longer the case with document notes. This can be mitigated using git version control, but also by making smart organizational choices of those documents where what gets deleted is trivial and need not be captured, or can be recovered by referring to the method described in the process notes. We can also create drafts of documents as well as we see fit.

# 6 Conclusion

The dual use of historical and document notes should bring us the **re** in **reproduction**. By giving us a space to play in, we can **re**discover, and **re**express. We have more power to delete, and thus play with the ideas considered. We can refine through addition and deletion, and open up our process notes to be less noisy and more focused on methodical choices as to how important changes came about.

We have also explored how explanations are a powerful type of note, and they have a dual history/document character to them, both of which help us understand the subject matter better and also our own methods by re-examining our own work for evidence to make judgments or re-explain how we encountered a problem and how we came to solve it and why.

If you have read this far, thank you for tuning in! Hopefully you can take something out of all the note taking tinkering here!

Until next time!

# 7 Related

Notes: [001 Writing for Historical and Document Notes](../../archived/2026-05-21_2026/topic/write/wiki/2026/000%20Notes%20for%20Posts/entries/001%20Writing%20for%20Historical%20and%20Document%20Notes.md)
