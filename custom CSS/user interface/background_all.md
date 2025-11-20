## Description

This background is slightly softer for the default main themes.
***

- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC

***


## Code

```css
/* Background for workspace */
/* Background variables for dark theme */
.theme-dark {
  --bg_dark2_x: 18, 18, 24;
  --bg_dark2: rgb(var(--bg_dark2_x));
  --bg_dark_x: 22, 22, 30;
  --bg_dark: rgb(var(--bg_dark_x));
  --bg_x: 26, 27, 38;
  --bg: rgb(var(--bg_x));
  --bg_highlight_x: 41, 46, 66;
  --bg_highlight: rgb(var(--bg_highlight_x));
  --bg_highlight_dark_x: 36, 40, 59;
  --bg_highlight_dark: rgb(var(--bg_highlight_dark_x));
}

/* Background variables for light theme */
.theme-light {
  --bg_dark2_x: 188, 189, 194;
  --bg_dark2: rgb(var(--bg_dark2_x));
  --bg_dark_x: 203, 204, 209;
  --bg_dark: rgb(var(--bg_dark_x));
  --bg_x: 213, 214, 219;
  --bg: rgb(var(--bg_x));
  --bg_highlight_x: 220, 222, 226;
  --bg_highlight: rgb(var(--bg_highlight_x));
  --bg_highlight_dark_x: 195, 197, 201;
  --bg_highlight_dark: rgb(var(--bg_highlight_dark_x));
}

/* Applying to Obsidian's standard variables */
.theme-dark,
.theme-light {
  --background-primary: var(--bg);
  --background-primary-alt: var(--bg);
  --background-secondary: var(--bg_dark);
  --background-secondary-alt: var(--bg_dark);
  
  --background-modifier-border: var(--bg_highlight);
  --background-modifier-border-focus: var(--bg_highlight);
  --background-modifier-border-hover: var(--bg_highlight);
  --background-modifier-form-field: var(--bg_dark);
  --background-modifier-form-field-highlighted: var(--bg_dark);
  --background-modifier-cover: rgba(var(--bg_dark_x), 0.8);
  --background-modifier-hover: var(--bg_highlight);
  --background-modifier-message: rgba(var(--bg_highlight_x), 0.9);
  --background-modifier-active-hover: var(--bg_highlight);
  
  --scrollbar-bg: var(--bg_dark2);
  --code-background: var(--bg_highlight_dark);
  --blockquote-background-color: var(--bg_dark);
  --table-header-background: var(--bg_dark2);
  --embed-background: var(--bg_dark);
}

/* Additional elements */
.is-mobile .suggestion-item.is-selected {
  background-color: var(--bg_highlight);
}

.notice {
  background-color: var(--bg_dark);
  border: 2px solid var(--bg_highlight);
}
```


