## Description


***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***


## How to use

```

```

## Code

```css

/*Custom status-bar*/
.app-container .status-bar {
  display: flex;
  position: fixed;
  bottom: 23vh;
  background: none !important;k
  z-index: var(--layer-cover);
  transform-origin:  right top;
  transform: rotate(90deg);
  border: none;
    /*border: orange 2px solid;*/
}

.app-container .status-bar .svg-icon {
    --icon-stroke: 1;
  display: inline-block;
  transform: rotate(-90deg) scale(1.3);
  
 /*border: 2px solid #ffb74d;*/
}

/*Improve click-area of Commander icon in status-bar*/
.cmdr.status-bar-item.clickable-icon {
  /*padding-left: 1em;*/
  padding-right: 1.5em;
  /*background: #ffb74d;*/
}

.theme-light .app-container .status-bar .svg-icon {
  color: #000000;
  opacity: 0.4;
}

.theme-dark .app-container .status-bar .svg-icon {
  color: #ffb74d;
  opacity: 0.7;
}
```


