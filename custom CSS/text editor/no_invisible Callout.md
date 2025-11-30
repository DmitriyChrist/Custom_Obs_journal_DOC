## Description

empty callout for js-block
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***

## Code

```css
/******************************/
/* Callout [!no] - empty callout
link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC*/
/******************************/

.callout[data-callout="no"] {
  background-color: transparent;
  --callout-radius: 0;
  --callout-title-padding: 0;
  --callout-content-padding: 0 !important;
}

.callout[data-callout="no"] .callout-title,
.callout[data-callout="no"] .callout-fold,
.callout[data-callout="no"] .callout-title-inner,
.callout[data-callout="no"] .callout-icon {
  display: none;
}

/* [!no] Callout - Complete Margin/Padding Reset */
.el-div .callout[data-callout="no"] .callout-content,
.el-div .callout[data-callout="no"] .callout-content * {
  margin-block: 0;
}

```


