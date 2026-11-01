# 1 Problem

For many purposes, we can view content directly in obsidian or through github by converting our obsidian vault to [CommonMark](https://spec.commonmark.org/0.31.2/).

However, sometimes we want a full-fledged website for a service, like a portfolio landing page or a website offering services to be sold.

We want to maintain a strict content-style separation and be able to automatically generate websites based on different contents from an obsidian vault.

# 2 Requirements

We need to make standards for how the content is created, how websites can be generated as a result, and how to customize the styling and function of the website or provide hooks for different website implementations to interface with obsidian markdown files. A lot of this should be possible via an obsidian plugin as part of this project.

We need to also configure a data directory for content generated through the website rather. Think for example user comments, or website database content. This is part of the website implementation and does not interact back with the obsidian vault, but it can be good for analytics or any future feature that requires them.

For a functioning demo, we need to provide six working items:

1. An obsidian plugin that can power the website-generation process
1. An obsidian vault with example content
1. A standard defining how website implementations source content from obsidian vaults
1. Two website implementation skeletons using different web technologies that generate a final website using the obsidian plugin and the example content.
1. Configuration source for obsidian vault conventions to be used by the obsidian plugin

# 3 Goals

* [ ] [000 First prototype of a publicly hosted obsidian-sourced website](archived/goals/2025/000%20First%20prototype%20of%20a%20publicly%20hosted%20obsidian-sourced%20website.md)
