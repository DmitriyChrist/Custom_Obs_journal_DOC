## Description

This snippet reduces the default editor padding in Reading, Preview and Source modes, expanding the overall workspace.
It also adjusts line number indentation from the screen edge and text, ensuring content remains within screen boundaries.
***

- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC

***
![](https://i.imgur.com/fckoJAt.jpeg)

## Code

```css
@scope (.workspace-split.mod-root) {
  :scope {
    
    --mobR-pI-left: 1vw;
    --mobR-pI-right: 1vw;
    --mobS-pI-left: 3vw;
    --mobS-pI-right: 2.5vw;
    --gutter-margin: 0vw;
  }

  @media (orientation: landscape) {
    :scope {
    --mobR-pI-left: 1vw;
    --mobR-pI-right: 1vw;
    --mobS-pI-left: 3vw;
    --mobS-pI-right: 3vw;
    --gutter-margin: 0vw;
    }
  }

  /* Adjusting the margins from the line number */
  .cm-gutters {
    margin-right: var(--gutter-margin) !important;
  }

  /* For source view */
  .markdown-source-view.mod-cm6 .cm-scroller {
    padding-inline: var(--mobS-pI-left) var(--mobS-pI-right) !important;
  }

  /* For Reading mode */
  .markdown-reading-view,
  .markdown-preview-view,
  .markdown-preview-section,
  .cm-content {
    padding-block: 0;
    padding-inline: var(--mobR-pI-left) var(--mobR-pI-right);
  }
}
```


