---
title: "Considerations for making a tree view component accessible"
date: 2025-01-29
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

A deep dive on the work that went into making the component that powers repository and pull request file trees.

The post Considerations for making a tree view component accessible appeared first on The GitHub Blog.

Tree views are a core part of the GitHub experience. You’ve encountered one if you’ve ever navigated through a repository’s file structure or reviewed a pull request.

![Browsing files on Primer's design repository. A tree view showing the repositories directory structure occupies a quarter of the screen. The other three quarters are taken up by the content of the content subdirectory. The tree view shows expanded and collapsed directories, as well as files nested at multiple levels of depth.](https://github.blog/wp-content/uploads/2025/01/github-repo-browser.png?w=1024&resize=1024%2C653)

On GitHub, a tree view is the list of folders and the files they contain. It is analogous to the directory structure your operating system uses as a way of organizing things.

Tree views are notoriously difficult to implement in an accessible way. This post is a deep dive into some of the major considerations that went into how we made GitHub’s tree view component accessible. We hope that it can be used as a reference and help others.

## Start with Windows

It’s important to have components with complex interaction requirements **map to something people are already familiar with using**. This allows for responsiveness to the keypresses they will try to navigate and take action on our tree view instances.

We elected to adopt Windows File Explorer’s tree view implementation, given the prominence of Windows’ usage for desktop screen reader users.

![A Windows 11 File Explorer window showing a tree view and a list of subdirectories that one of its folders contains. The tree view demonstrates how the C drive contains multiple nested folders to organize its content.](https://github.blog/wp-content/uploads/2025/01/windows-file-explorer.png?w=1024&resize=1024%2C783)

Navigating and taking actions on items in Windows’ tree view using NVDA and JAWS helped us get a better understanding of how things worked, including factors such as focus management, keyboard shortcuts, and expected assistive technology announcements.

## Then maybe reference the APG

The ARIA Authoring Practices Guide (APG) is a bit of an odd artifact. It looks official but is no longer recognized by the W3C as a formal document.

This is to say that the APG can serve as a helpful high-level resource for things to consider for your overall approach, but its suggestions for code necessitate deeper scrutiny.

## Build from a solid, semantic foundation

At its core, a tree view is a list of lists. Because of this, we used `ul` and `li` elements for parent and child nodes:

```
<ul>
  <li>
    <ul>
      <li>.github/</li>
      <li>source/</li>
      <li>test/</li>
    </ul>
  </li>
  <li>.gitignore</li>
  <li>README.md</li>
</ul>
```

There are a few reasons for doing this, but the main considerations are:

- Better assurance that a meaningful accessibility tree is generated,
- Lessening the work we need for future maintenance, and consequential re-verification that our updates continue to work properly, and
- Better guaranteed interoperability between different browsers, apps, and other technologies.

**NOTE**: GitHub currently does not virtualize its file trees. We would need to revisit this architectural decision if this ever changes.

### Better broad assistive technology support

The more complicated an interactive pattern is, the greater the risk that there are bugs or gaps with assistive technology support.

Given the size of the audience GitHub serves, it’s important that we consider more than just majority share assistive technology considerations.

We found that utilizing semantic HTML elements also performed better for some less-common assistive technologies. This was especially relevant with some lower-power devices, like an entry-level Android smartphone from 2021.

### Better Forced Color Mode support

Semantic HTML elements also map to native operating system UI patterns, meaning that Forced Color Mode’s heuristics will recognize them without any additional effort. This is helpful for people who rely on the mode to see screen content.

The heuristic mapping behavior does not occur if we used semantically neutral `div` or `span` elements, and would have to be manually recreated and maintained.

## Use a composite widget

A composite widget allows a component that contains multiple interactive elements to only require one tab stop unless someone chooses to interact with it further.

Consider a file tree for a repository that contains 500+ files in 20+ directories. Without a composite widget treatment, someone may have to press Tab far too many times to bypass the file tree component and get what they need.

## Think about wrapping it in a landmark

Like using a composite widget, landmark regions help some people quickly and efficiently navigate through larger overall sections of the page. Because of this, we wrapped the entire file tree in a `nav` landmark element.

This does not mean every tree view component should be a landmark, however! We made this decision for the file tree because it is frequently interacted with as a way to navigate through a repository’s content.

## Go with a roving `tabindex` approach

A roving tabindex is a technique that uses `tabindex="-1"` applied to each element in a series, and then updates the `tabindex` value to use `0` instead in response to user keyboard input. This allows someone to traverse the series of elements, as focus “roves” to follow their keypresses.

```
<li tabindex="-1">File 1</li>
<li tabindex="-1">File 2</li>
<li tabindex="0">File 3</li>
<li tabindex="-1">File 4</li>
```

The roving `tabindex` approach performed better than utilizing `aria-activedescendant`, which had issues with VoiceOver on macOS and iOS.

## Enhance with ARIA

We use a considered set of ARIA declarations to build off our semantic foundation.

Note that while we intentionally started with semantic HTML, there are certain ARIA declarations that are needed. The use of ARIA here is necessary and intentional, as it expands the capabilities of HTML to describe something that HTML alone cannot describe—a tree view construct.

Our overall approach follows what the APG suggests, in that we use the following:

- `role="tree"` is placed on the parent `ul` element, to communicate that it is a tree view construct.
- `role="treeitem"` is placed on the child `li` elements, to communicate that they are tree view nodes.
- `role="group"` is declared on child `ul` elements, to communicate that they contain branch and leaf nodes.
- `aria-expanded` is declared on directories, with a value of `true` to communicate that the branch node is in an opened state and a value of `false` to communicate that it is in a collapsed state instead.
- `aria-selected` is used to indicate if branch or leaf nodes have been chosen by user navigation, and can therefore have user actions applied to them.

We also made the following additions:

- `aria-hidden="true"` is applied to SVG icons (folders, files, etc.) to ensure its content is not announced.
- `aria-current="true"` is placed on the selected node to better support when a node is deep linked to via URL.

**NOTE**: We use “branch node” and “leaf node” as broad terms that can apply to all tree view components we use on GitHub. For the file tree, branch nodes would correspond to directories and subdirectories, and leaf nodes would correspond to files.

## Support expected navigation techniques

The following behaviors are what people will try when operating a tree view construct, so we support them:

### Keyboard keypresses

- Tab: Places focus on the entire tree view component, then moves focus to the next focusable item on the view.
- Enter:
    - If a branch node is selected: Displays the directory’s contents.
    - If a leaf node is selected: Displays the leaf node’s contents.
- Down: Moves selection to the next node that can be selected without opening or closing a node.
- Up: Moves selection to the previous node that can be selected without opening or closing a node.
- Right:
    - If a branch node is selected and in a collapsed state: Expands the selected collapsed branch node and does not move selection.
    - If a branch node is selected and in an expanded state: Moves selection to the directory’s first child node.
- Left:
    - If a branch node is selected and in an expanded state: Collapses the selected collapsed directory node and does not move selection.
    - If a branch node is selected and in a collapsed state: Moves selection to the node’s parent directory.
    - If a leaf node is selected: Moves selection to the leaf node’s parent directory.
- End: Moves selection to the last node that can be selected.
- Home: Moves selection to the first node that can be selected.

We also support **typeahead selection**, as we are modeling Windows File Explorer’s tree view behaviors. Here, we move selection to the node closest to the currently selected node whose name matches what the user types.

### Middle clicking

Nodes on tree view constructs are tree items, not links. Because of this, tree view nodes do not support the behaviors you get with using an anchor element, such as opening its URL in a new tab or window.

We use JavaScript to listen for middle clicks and Control+Enter keypresses to replicate this behavior.

## Consider states

### Loading

Tree views on GitHub can take time to retrieve their content, and we may not always know how much content a branch node contains.

![A directory called, 'src' that is selected and in an expanded that. It contains a single leaf node that contains a loading spinner with a label of 'Loading…".](https://github.blog/wp-content/uploads/2025/01/tree-view-loading-nodes.png?w=890&resize=890%2C282)

Live region announcements are tricky to get right, but integral to creating an equivalent experience. We use the following announcements:

- If there is a known amount of nodes that load, we enumerate the incoming content with an announcement that reads, “Loading {x} items.”
- If there is an unknown number of nodes that load, we instead use a more generic announcement of, “Loading…”
- If there are no nodes that load we use an announcement message that reads, “{branch node name} is empty.”

Additionally, we manage focus for loading content:

- If focus is placed on a placeholder loading node when the content loads in: Move focus from the placeholder node to the first child node in the branch node.
- If focus is on a placeholder loading node but the branch node does not contain content: Move focus back to the branch node. Additionally, we remove the branch node’s `aria-expanded` declaration.

### Errors

Circumstances can conspire to interfere with a tree view component’s intended behavior. Examples of this could be a branch node failing to retrieve content or a partial system outage.

In these scenarios, the tree view component will use a straightforward dialog component to communicate the error.

## Fix interoperability issues

As previously touched on, complicated interaction patterns run the risk of compatibility issues. Because of this, it’s essential to test your efforts with actual assistive technology to **ensure it actually works**.

We made the following adjustments to provide better assistive technology support:

### Use `aria-level`

Screen readers can report on the depth of a nested list item. For example, a `li` element placed inside of a `ul` element nested three levels deep can announce itself as such.

We found that we needed to explicitly declare the level on each `li` element to recreate this behavior for a tree view. For our example, we’d also need to set `aria-level="3"` on the `li` element.

This fix addressed multiple forms of assistive technology we tested with.

### Explicitly set the node’s accessible name on the `li` element

A node’s accessible name is typically set by the text string placed inside the `li` element:

```
<li>README.md</li>
```

However, we found that VoiceOver on macOS and iOS did not support this. This may be because of the relative complexity of each node’s inner DOM structure.

We used `aria-labelledby` to get around this problem, with a value that pointed to the `id` set on the text portion of each node:

```
<li aria-labelledby="readme-md">
  <div>
   <!-- Icon -->
  </div>
  <div id="readme-md">
    README.md
  </div>
</li>
```

This guarantees that:

- the node’s accessible name is announced when focus is placed on the `li` element,
- and that the announcement matches what is shown visually.

## Where we’d like to go from here

There’s a couple areas we’re prototyping and iterating on to better serve our users:

### Supporting links inside a node

Browsers apply a lot of behaviors to anchor elements, such as the ability to copy the URL.

We’d like to replace the JavaScript that listens for middle clicks with a more robust native solution, only without sacrificing interoperability and assistive technology support.

### Supporting multiple actions per node

Tree views constructs were designed assuming a user will only ever navigate to a node and activate it.

GitHub has use cases that require actions other than activating the node, and we’re exploring how to accomplish that. This is exciting, as it represents an opportunity to evolve the tree view construct on the web.

## Always learning

An accessible tree view is a complicated component to make, and it requires a lot of effort and testing to get right. However, this work helps to ensure that everyone can use a core part of GitHub, regardless of device, circumstance, or ability.

We hope that highlighting the considerations that went into our work can help you on your accessibility journey.  

**Share your experience**: We’d love to hear from you if you’ve run into issues using our tree view component with assistive technology. This feedback is invaluable to helping us continue to make GitHub more accessible.

The post Considerations for making a tree view component accessible appeared first on The GitHub Blog.

Go to Source
