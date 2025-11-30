## Description


***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***
![](https://i.imgur.com/kHDCx8D.jpeg)

## Code

```css
/* MULTILAYER CARD HEADINGS */
/* Shared styles for both preview and source mode in dark and light themes */
.markdown-preview-view h1,
.markdown-preview-view h2,
.markdown-preview-view h3,
.markdown-preview-view h4,
.markdown-preview-view h5,
.markdown-preview-view h6,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-1,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-2,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-3,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-4,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-5,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-6 {
  padding: 0.8rem 1.5rem !important;
  font-style: italic;
  text-align: center;
  color: var(--text-title);
  border: 3px solid;
  border-radius: 8px;
  /* Soft drop shadow and layered effect */
  box-shadow:
    0 1px 0 rgba(255, 255, 255, 0.08) inset,
    0 4px 8px rgba(0, 0, 0, 0.19),
    0 8px 0 -3px var(--layer-bg, rgba(0,0,0,0.1)),
    0 8px 1px -3px rgba(0,0,0,0.12),
    0 16px 0 -6px var(--layer-bg2, rgba(0,0,0,0.04)),
    0 16px 1px -6px rgba(0,0,0,0.09);
}

/* DARK THEME COLOR VARIATIONS AND BACKGROUNDS */
.theme-dark .markdown-preview-view h1,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-1 {
  --layer-bg: rgba(122, 162, 247, 0.5);
  --layer-bg2: rgba(122, 162, 247, 0.3);
  border-color: rgb(122, 162, 247);
  background: linear-gradient(135deg, rgba(122, 162, 247, 0.2) 0%, var(--bg_highlight_dark) 100%);
  font-size: 2rem;
}
.theme-dark .markdown-preview-view h2,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-2 {
  --layer-bg: rgba(187, 154, 247, 0.5);
  --layer-bg2: rgba(187, 154, 247, 0.3);
  border-color: rgb(187, 154, 247);
  background: linear-gradient(135deg, rgba(187, 154, 247, 0.2) 0%, var(--bg_highlight_dark) 100%);
  font-size: 1.8rem;
}
.theme-dark .markdown-preview-view h3,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-3 {
  --layer-bg: rgba(125, 207, 255, 0.5);
  --layer-bg2: rgba(125, 207, 255, 0.3);
  border-color: rgb(125, 207, 255);
  background: linear-gradient(135deg, rgba(125, 207, 255, 0.2) 0%, var(--bg_highlight_dark) 100%);
  font-size: 1.6rem;
}
.theme-dark .markdown-preview-view h4,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-4 {
  --layer-bg: rgba(158, 206, 106, 0.5);
  --layer-bg2: rgba(158, 206, 106, 0.3);
  border-color: rgb(158, 206, 106);
  background: linear-gradient(135deg, rgba(158, 206, 106, 0.2) 0%, var(--bg_highlight_dark) 100%);
  font-size: 1.4rem;
}
.theme-dark .markdown-preview-view h5,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-5 {
  --layer-bg: rgba(255, 158, 100, 0.5);
  --layer-bg2: rgba(255, 158, 100, 0.3);
  border-color: rgb(255, 158, 100);
  background: linear-gradient(135deg, rgba(255, 158, 100, 0.2) 0%, var(--bg_highlight_dark) 100%);
  font-size: 1.2rem;
}
.theme-dark .markdown-preview-view h6,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-6 {
  --layer-bg: rgba(224, 175, 104, 0.5);
  --layer-bg2: rgba(224, 175, 104, 0.3);
  border-color: rgb(224, 175, 104);
  background: linear-gradient(135deg, rgba(224, 175, 104, 0.2) 0%, var(--bg_highlight_dark) 100%);
  font-size: 1rem;
}


/* LIGHT THEME COLOR VARIATIONS AND BACKGROUNDS */
.theme-light .markdown-preview-view h1,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-1 {
  --layer-bg: rgba(122, 162, 247, 0.18);
  --layer-bg2: rgba(122, 162, 247, 0.10);
  border-color: rgb(61, 89, 161);
  background: linear-gradient(135deg, rgba(122, 162, 247, 0.11) 0%, var(--bg_highlight_light, #fff) 100%);
  color: rgb(61, 89, 161);
  font-size: 2rem;
}
.theme-light .markdown-preview-view h2,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-2 {
  --layer-bg: rgba(187, 154, 247, 0.18);
  --layer-bg2: rgba(187, 154, 247, 0.10);
  border-color: rgb(147, 114, 207);
  background: linear-gradient(135deg, rgba(187, 154, 247, 0.11) 0%, var(--bg_highlight_light, #fff) 100%);
  color: rgb(147, 114, 207);
  font-size: 1.8rem;
}
.theme-light .markdown-preview-view h3,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-3 {
  --layer-bg: rgba(125, 207, 255, 0.18);
  --layer-bg2: rgba(125, 207, 255, 0.10);
  border-color: rgb(85, 167, 215);
  background: linear-gradient(135deg, rgba(125, 207, 255, 0.11) 0%, var(--bg_highlight_light, #fff) 100%);
  color: rgb(85, 167, 215);
  font-size: 1.6rem;
}
.theme-light .markdown-preview-view h4,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-4 {
  --layer-bg: rgba(158, 206, 106, 0.18);
  --layer-bg2: rgba(158, 206, 106, 0.10);
  border-color: rgb(118, 166, 66);
  background: linear-gradient(135deg, rgba(158, 206, 106, 0.11) 0%, var(--bg_highlight_light, #fff) 100%);
  color: rgb(118, 166, 66);
  font-size: 1.4rem;
}
.theme-light .markdown-preview-view h5,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-5 {
  --layer-bg: rgba(255, 158, 100, 0.18);
  --layer-bg2: rgba(255, 158, 100, 0.10);
  border-color: rgb(215, 118, 60);
  background: linear-gradient(135deg, rgba(255, 158, 100, 0.11) 0%, var(--bg_highlight_light, #fff) 100%);
  color: rgb(215, 118, 60);
  font-size: 1.2rem;
}
.theme-light .markdown-preview-view h6,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-6 {
  --layer-bg: rgba(224, 175, 104, 0.18);
  --layer-bg2: rgba(224, 175, 104, 0.10);
  border-color: rgb(184, 135, 64);
  background: linear-gradient(135deg, rgba(224, 175, 104, 0.11) 0%, var(--bg_highlight_light, #fff) 100%);
  color: rgb(184, 135, 64);
  font-size: 1rem;
}

/* END Multilayer card headings */
```


