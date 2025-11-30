## Description

Changes the default visual indicators for nested folder hierarchy levels

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- initial idea: https://forum.obsidian.md/t/add-rule-lines-to-folder-tree/19356
***
![](https://i.imgur.com/l17wV2z.jpeg)
## Code

```css
/*******************/
/* modified lines of folder hierarchy levels */
/************************/

:root {
  --on: initial;
  --off: /*!*/;
  
  --_1: var(--on);
  --_2: var(--off);
  --_3: var(--off);
}

.nav-folder-children {
  --1: var(--_1);
  --2: var(--_2);
  --3: var(--_3);
  
  border-left: 1px solid 
    var(--1, rgba(30, 161, 152, 1))
    var(--2, rgba(255, 0, 0, 1))
    var(--3, rgba(50, 115, 182, 1));
}

.nav-folder-children > * {
  --_1: var(--3);
  --_2: var(--1);
  --_3: var(--2);
}
```


