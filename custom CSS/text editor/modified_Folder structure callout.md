## Description


***
- author: BunkerD
- source: https://forum.obsidian.md/t/a-callout-for-a-folder-structure/61366
- modified: DOChrist
link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- fix: conflict with global variables
---
- Before:
![](https://i.imgur.com/BndqHMj.png)
- After:
![](https://i.imgur.com/w8YMzQG.jpeg)
## How to use

```md
> [!folder] folder
>
> - 📁 Folder _`           `← root folder_
>   - 📁 Subfolder 1 _`    `← another folder_
>     - 📄 Subitem 1-1
>     - 📄 Subitem 1-2
>   - 📁 Subfolder 2
>     - 📄 Subitem 2-1
>     - 📄 Subitem 2-2
>     - 📁 Subsubfolder
>       - 📄 ==Name==.jpg _`   `… Some variable name?_
>       - …
```

## Code

```css
/*
author: BunkerD
source: https://forum.obsidian.md/t/a-callout-for-a-folder-structure/61366

modified: DOChrist
link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
*/

/* === FOLDER CALLOUT - MINIMAL GLOBAL LIST OVERRIDE === */

.callout[data-callout="folder" i] {
  --list-indent: 0em;
  --indentation-guide-color: transparent;
  --callout-color: #4ade80;
  overflow: scroll;
  background-color: transparent;
}

/* 2. switch-off indentation guides*/ 
.callout[data-callout="folder" i] .cm-indent::before,
.callout[data-callout="folder" i] li > ul::before,
.callout[data-callout="folder" i] li > ol::before {
    border: none;
}

.callout[data-callout="folder" i] .callout-icon .svg-icon {
  display: none;
}

.callout[data-callout="folder" i] .callout-icon::after {
  content: "📁"; 
  scale: 1.2;
  padding-inline-end: 1rem;
  padding-block: 0.5rem;
  color: var(--callout-color);
  text-shadow: 
    0 0 5px var(--callout-color),
    0 0 10px var(--callout-color),
    0 0 15px var(--callout-color);
  filter: drop-shadow(0 0 3px var(--callout-color));
  animation: neonPulse 2s infinite;

@keyframes neonPulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.6; }
}
}

.callout[data-callout="folder" i] .callout-fold {
  color: var(--callout-color);
}

.callout[data-callout="folder" i] .callout-title-inner {
  position: relative;
  padding-inline-start: 1rem;
  padding-block-end: 0.4rem;
  font-size: 1.2rem;
  color: var(--callout-color);
  border-inline-start: 2px solid var(--callout-color);
}

.callout[data-callout="folder" i] .callout-title-inner::before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 70vw;
  height: 2px;
  background: linear-gradient(
    to right,
    var(--callout-color),
    transparent
  );
}
 
.callout[data-callout="folder" i] .list-bullet {
  display: none;
}
.callout[data-callout="folder" i] ul {
  -webkit-margin-after: 0;
  margin-block-end: 0;
}
.callout[data-callout="folder" i] li {
  --list-indent: 1.5em;
  list-style-type: none;
  font-family: var(--font-monospace);
  white-space: nowrap;
}
.callout[data-callout="folder" i] li li {
  position: relative;
  box-sizing: border-box;
}
.callout[data-callout="folder" i] li li::before,
.callout[data-callout="folder" i] li li::after {
  content: "";
  position: absolute;
  left: -1em;
  background: var(--list-marker-color);
}
.callout[data-callout="folder" i] li li::before {
  top: 0.85em;
  width: 10px;
  height: 1.5px;
  margin: auto;
}
.callout[data-callout="folder" i] li li::after {
  top: 0;
  bottom: 0;
  width: 1.5px;
  height: 100%;
}
.callout[data-callout="folder" i] li li:last-child::after {
  height: 0.85em;
}
.callout[data-callout="folder" i] li .list-collapse-indicator {
  position: absolute;
  left: -2em;
  margin: 0;
}
.callout[data-callout="folder" i] li em {
  opacity: 0.5;
  font-family: var(--font-text);
}
.callout[data-callout="folder" i] li code {
  font-size: 1em;
  font-family: var(--font-monospace);
  font-style: normal !important;
  background: none;
  white-space: pre;
  margin: 0;
  padding: 0;
}
.callout[data-callout="folder" i] li mark {
  font-style: italic;
  color: #d3a3e5;
}

.callout[data-callout="folder" i] .callout-content {
  overflow-x: visible;
}
```

