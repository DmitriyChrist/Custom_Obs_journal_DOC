
#### Description

In Preview mode, it blocks editing of callouts

***

- author: last version from ariehen
- link: https://forum.obsidian.md/t/lock-callouts-edit-mode/94686

***

#### Code

```css
.is-live-preview .callout {
    border: 1px solid orange;
}

/* === OBSIDIAN CALLOUT CLICK PREVENTION - SIMPLE & UNIVERSAL === */

/* Disable clicks on ALL callout backgrounds */
.is-live-preview .cm-embed-block.cm-callout {
    pointer-events: none !important;
}

/* === EDIT BUTTON AND CALLOUT CONTROLS === */
/* Enable edit button and callout controls */
.is-live-preview .cm-embed-block.cm-callout .edit-block-button,
.is-live-preview .cm-embed-block.cm-callout .cm-edit-block-button,
.is-live-preview .cm-embed-block.cm-callout .callout-fold,
.is-live-preview .cm-embed-block.cm-callout .callout-title .clickable-icon,
.is-live-preview .cm-embed-block.cm-callout .callout-title .clickable-icon[aria-label*="Edit"] {
    pointer-events: auto !important;
    cursor: pointer !important;
}

/* Enable callout title for edit button access */
.is-live-preview .cm-embed-block.cm-callout .callout-title {
    pointer-events: auto !important;
}

/* Disable title text to prevent accidental edit mode */
.is-live-preview .cm-embed-block.cm-callout .callout-title .callout-title-inner {
    pointer-events: none !important;
}

/* === BASIC INTERACTIVE ELEMENTS === */
/* Enable links */
.is-live-preview .cm-embed-block.cm-callout a,
.is-live-preview .cm-embed-block.cm-callout .internal-link,
.is-live-preview .cm-embed-block.cm-callout .external-link {
    pointer-events: auto !important;
    cursor: pointer !important;
}

/* Enable form elements */
.is-live-preview .cm-embed-block.cm-callout input,
.is-live-preview .cm-embed-block.cm-callout select,
.is-live-preview .cm-embed-block.cm-callout textarea,
.is-live-preview .cm-embed-block.cm-callout button {
    pointer-events: auto !important;
    cursor: pointer !important;
}

/* Enable checkboxes specifically */
.is-live-preview .cm-embed-block.cm-callout input[type="checkbox"] {
    pointer-events: auto !important;
    cursor: pointer !important;
}

/* === DATAVIEW ELEMENTS === */
/* Enable dataview rendered content and tables */
.is-live-preview .cm-embed-block.cm-callout .dataview-result-list-root-node,
.is-live-preview .cm-embed-block.cm-callout .dataview-result-list-root-node *,
.is-live-preview .cm-embed-block.cm-callout table,
.is-live-preview .cm-embed-block.cm-callout table * {
    pointer-events: auto !important;
}

/* Re-enable links within dataview */
.is-live-preview .cm-embed-block.cm-callout .dataview a,
.is-live-preview .cm-embed-block.cm-callout .dataview .internal-link,
.is-live-preview .cm-embed-block.cm-callout .dataview .external-link {
    pointer-events: auto !important;
    cursor: pointer !important;
}

/* === CHARTS AND VISUALIZATIONS === */
/* Enable canvas and SVG elements */
.is-live-preview .cm-embed-block.cm-callout canvas,
.is-live-preview .cm-embed-block.cm-callout svg {
    pointer-events: auto !important;
    cursor: default !important;
}

/* === MEDICATION HEATMAP === */
/* Enable heatmap container and all children */
.is-live-preview .cm-embed-block.cm-callout [id^="medication-heatmap"],
.is-live-preview .cm-embed-block.cm-callout [id^="medication-heatmap"] *,
.is-live-preview .cm-embed-block.cm-callout [id*="medication-heatmap"],
.is-live-preview .cm-embed-block.cm-callout [id*="medication-heatmap"] * {
    pointer-events: auto !important;
}

/* Enable heatmap squares for tooltips */
.is-live-preview .cm-embed-block.cm-callout .heatmap-square {
    pointer-events: auto !important;
    cursor: help !important;
}

/* Enable year filter buttons */
.is-live-preview .cm-embed-block.cm-callout .year-filter-btn {
    pointer-events: auto !important;
    cursor: pointer !important;
}

/* === CALENDAR AND DATE PICKERS === */
/* Enable Flatpickr calendar */
.is-live-preview .cm-embed-block.cm-callout .flatpickr-calendar,
.is-live-preview .cm-embed-block.cm-callout .flatpickr-calendar * {
    pointer-events: auto !important;
    cursor: pointer !important;
}

/* === GENERAL INTERACTIVE ELEMENTS === */
/* Enable common interactive elements */
.is-live-preview .cm-embed-block.cm-callout .clickable,
.is-live-preview .cm-embed-block.cm-callout [onclick],
.is-live-preview .cm-embed-block.cm-callout [role="button"],
.is-live-preview .cm-embed-block.cm-callout .interactive,
.is-live-preview .cm-embed-block.cm-callout [data-year],
.is-live-preview .cm-embed-block.cm-callout [data-toggle],
.is-live-preview .cm-embed-block.cm-callout [data-action] {
    pointer-events: auto !important;
    cursor: pointer !important;
}

/* === TOOLBAR ELEMENTS === */
/* Enable toolbar containers and buttons */
.is-live-preview .cm-embed-block.cm-callout .cg-note-toolbar-callout {
    pointer-events: auto !important;
}

.is-live-preview .cm-embed-block.cm-callout .cg-note-toolbar-callout button,
.is-live-preview .cm-embed-block.cm-callout .cg-note-toolbar-callout .clickable-icon {
    pointer-events: auto !important;
    cursor: pointer !important;
}

/* === CUSTOM CHART CONTAINERS === */
/* Enable any custom chart containers */
.is-live-preview .cm-embed-block.cm-callout .my-graph-container,
.is-live-preview .cm-embed-block.cm-callout .my-graph-container *,
.is-live-preview .cm-embed-block.cm-callout .chart-container,
.is-live-preview .cm-embed-block.cm-callout .chart-container * {
    pointer-events: auto !important;
}

/* === TEXT SELECTION === */
/* Disable text selection on callout content to prevent accidental selection */
.is-live-preview .cm-embed-block.cm-callout .callout-content {
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}

/* Re-enable text selection for specific elements */
.is-live-preview .cm-embed-block.cm-callout .callout-content table,
.is-live-preview .cm-embed-block.cm-callout .callout-content pre,
.is-live-preview .cm-embed-block.cm-callout .callout-content code,
.is-live-preview .cm-embed-block.cm-callout .callout-content input[type="text"],
.is-live-preview .cm-embed-block.cm-callout .callout-content textarea {
    -webkit-user-select: text;
    -moz-user-select: text;
    -ms-user-select: text;
    user-select: text;
}
```


