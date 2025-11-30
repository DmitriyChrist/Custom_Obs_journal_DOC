## Description

I don't remember the source of this code, but on the phone, it's simply a must-have.

- Pros: convenient for collapsing and expanding headings
- Cons: it makes links and footnotes in the heading unclickable.
***
- author:
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***

## Code

```css
/* 1. Make the entire header area clickable */
.el-h1 h1 {
    position: relative;
}

/* 2. Make the original arrow invisible, but still clickable */
.heading-collapse-indicator {
    position: absolute;
    left: 0;
    width: 100%;
    height: 30px;
    z-index: 1;
}
```


