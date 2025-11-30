## Description


***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***

![](https://i.imgur.com/1Idyav2.jpeg)
## Code

```css

/* Double borders with indication */

/* Common styles for all headings */
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
  border-top: 3px double var(--heading-color);
  border-bottom: 3px double var(--heading-color);
  padding: 1
  rem 1rem !important;
  font-style: italic;
  text-align: center;
  color: var(--text-title);
  position: relative;
}



.markdown-preview-view h1::before,
.markdown-preview-view h2::before,
.markdown-preview-view h3::before,
.markdown-preview-view h4::before,
.markdown-preview-view h5::before,
.markdown-preview-view h6::before,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-1::before,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-2::before,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-3::before,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-4::before,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-5::before,
.markdown-source-view.mod-cm6 .cm-line.HyperMD-header-6::before {
  position: absolute;
  left: 2vw;
  bottom: 1vh;
  font-size: 0.6em;
  font-weight: bold;
  color: var(--heading-color);
  opacity: 0.6;
  font-style: normal;
}


/* ========== DARK THEME ========== */

.theme-dark .markdown-preview-view h1,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-1 {
  --heading-color: rgb(122, 162, 247);
  background: linear-gradient(90deg, rgba(122, 162, 247, 0.05) 0%, rgba(122, 162, 247, 0.2) 50%, rgba(122, 162, 247, 0.05) 100%);
  font-size: 2rem;
}

.theme-dark .markdown-preview-view h1::before,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-1::before {
  content: "I";
}

.theme-dark .markdown-preview-view h2,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-2 {
  --heading-color: rgb(187, 154, 247);
  background: linear-gradient(90deg, rgba(187, 154, 247, 0.05) 0%, rgba(187, 154, 247, 0.2) 50%, rgba(187, 154, 247, 0.05) 100%);
  font-size: 1.8rem;
}

.theme-dark .markdown-preview-view h2::before,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-2::before {
  content: "II";
}

.theme-dark .markdown-preview-view h3,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-3 {
  --heading-color: rgb(125, 207, 255);
  background: linear-gradient(90deg, rgba(125, 207, 255, 0.05) 0%, rgba(125, 207, 255, 0.2) 50%, rgba(125, 207, 255, 0.05) 100%);
  font-size: 1.6rem;
}

.theme-dark .markdown-preview-view h3::before,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-3::before {
  content: "III";
}

.theme-dark .markdown-preview-view h4,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-4 {
  --heading-color: rgb(158, 206, 106);
  background: linear-gradient(90deg, rgba(158, 206, 106, 0.05) 0%, rgba(158, 206, 106, 0.2) 50%, rgba(158, 206, 106, 0.05) 100%);
  font-size: 1.4rem;
}

.theme-dark .markdown-preview-view h4::before,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-4::before {
  content: "IV";
}

.theme-dark .markdown-preview-view h5,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-5 {
  --heading-color: rgb(255, 158, 100);
  background: linear-gradient(90deg, rgba(255, 158, 100, 0.05) 0%, rgba(255, 158, 100, 0.2) 50%, rgba(255, 158, 100, 0.05) 100%);
  font-size: 1.2rem;
}

.theme-dark .markdown-preview-view h5::before,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-5::before {
  content: "V";
}

.theme-dark .markdown-preview-view h6,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-6 {
  --heading-color: rgb(224, 175, 104);
  background: linear-gradient(90deg, rgba(224, 175, 104, 0.05) 0%, rgba(224, 175, 104, 0.2) 50%, rgba(224, 175, 104, 0.05) 100%);
  font-size: 1rem;
}

.theme-dark .markdown-preview-view h6::before,
.theme-dark .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-6::before {
  content: "VI";
}

/* ========== LIGHT THEME ========== */



.theme-light .markdown-preview-view h1,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-1 {
  --heading-color: rgb(61, 89, 161);
  background: linear-gradient(90deg, rgba(122, 162, 247, 0.1) 0%, rgba(122, 162, 247, 0.2) 50%, rgba(122, 162, 247, 0.1) 100%);
  font-size: 2rem;
}

.theme-light .markdown-preview-view h1::before,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-1::before {
  content: "I";
}

.theme-light .markdown-preview-view h2,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-2 {
  --heading-color: rgb(147, 114, 207);
  background: linear-gradient(90deg, rgba(187, 154, 247, 0.1) 0%, rgba(187, 154, 247, 0.2) 50%, rgba(187, 154, 247, 0.1) 100%);
  font-size: 1.8rem;
}

.theme-light .markdown-preview-view h2::before,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-2::before {
  content: "II";
}

.theme-light .markdown-preview-view h3,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-3 {
  --heading-color: rgb(85, 167, 215);
  background: linear-gradient(90deg, rgba(125, 207, 255, 0.1) 0%, rgba(125, 207, 255, 0.2) 50%, rgba(125, 207, 255, 0.1) 100%);
  font-size: 1.6rem;
}

.theme-light .markdown-preview-view h3::before,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-3::before {
  content: "III";
}

.theme-light .markdown-preview-view h4,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-4 {
  --heading-color: rgb(118, 166, 66);
  background: linear-gradient(90deg, rgba(158, 206, 106, 0.1) 0%, rgba(158, 206, 106, 0.2) 50%, rgba(158, 206, 106, 0.1) 100%);
  font-size: 1.4rem;
}

.theme-light .markdown-preview-view h4::before,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-4::before {
  content: "IV";
}

.theme-light .markdown-preview-view h5,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-5 {
  --heading-color: rgb(215, 118, 60);
  background: linear-gradient(90deg, rgba(255, 158, 100, 0.1) 0%, rgba(255, 158, 100, 0.2) 50%, rgba(255, 158, 100, 0.1) 100%);
  font-size: 1.2rem;
}

.theme-light .markdown-preview-view h5::before,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-5::before {
  content: "V";
}

.theme-light .markdown-preview-view h6,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-6 {
  --heading-color: rgb(184, 135, 64);
  background: linear-gradient(90deg, rgba(224, 175, 104, 0.1) 0%, rgba(224, 175, 104, 0.2) 50%, rgba(224, 175, 104, 0.1) 100%);
  font-size: 1rem;
}

.theme-light .markdown-preview-view h6::before,
.theme-light .markdown-source-view.mod-cm6 .cm-line.HyperMD-header-6::before {
  content: "VI";
}
```


