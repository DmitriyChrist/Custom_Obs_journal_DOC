---
link: https://forum.obsidian.md/t/mermaid-dark-light-theme-css-snippet-adding-contrast-providing-template/73147
---
Hi all,
I’d like to share my solution for a problem that I faced with Mermaid graphs in Obsidian - low contrast, hard to read, especially in dark theme. Please find below the css-snippet for Obsidian (Settings - Appearance - CSS Snippets).
```

.mermaid svg {
	max-width: 100%;
	height: auto;
}

body.theme-light .mermaid .cluster rect
{
	stroke-width: 1px !important;
	stroke: var(--color-base-60) !important;
	fill: var(--color-base-05) !important;
} 
body.theme-light .mermaid .node * {
	stroke-width: 1px !important;
	stroke: var(--color-base-60) !important;
	fill: var(--color-base-25) !important;
}
body.theme-dark .mermaid .edgeLabel {
	background-color: var(--color-base-05) !important;
}


body.theme-dark .mermaid .cluster rect
{
	stroke-width: 1px !important;
	stroke: var(--color-base-60) !important;
	fill: var(--color-base-30) !important;
} 
body.theme-dark .mermaid .node * {
	stroke-width: 1px !important;
	stroke: var(--color-base-60) !important;
	fill: var(--color-base-20) !important;
}
body.theme-dark .mermaid .edgeLabel {
	background-color: var(--color-base-30) !important;
}

```