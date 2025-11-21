## Description

This snippet vertically rotates the headerNavBar.
It removes the Reading-mode and Bookmark icons along with the file name area.
It also rotates the Commander plugin icons vertically.
***
- author: DOChist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***
![vertHeaderNavBar](attachments/vertHeaderNavBar.jpg)

## Code

```css

.is-mobile .view-header > * {
  display: none !important;
}

.is-mobile .view-header {
  display: flex;
  position: fixed;
  right: 10vw;
  background: none;
  transform-origin:  right top;
  transform: rotate(-90deg);
  /*border: orange 2px solid;*/
}

.is-mobile .view-header-nav-buttons,
.is-mobile .view-header > :nth-child(3) {
  display: flex !important;
  /*border: 1px orange solid;*/
}

.is-mobile .view-header-nav-buttons,
.is-mobile .view-header .view-action:nth-last-child(3) {
  display: flex !important;
}

.is-mobile .view-header-nav-buttons,
.is-mobile .view-header .view-action:last-child {
  border: 1px orange solid;
  border-radius: 5px;
  padding: 0px;
  opacity: 0.7;
}

.is-mobile .view-header-nav-buttons,
.is-mobile .view-header .view-action:nth-last-child(2),
.is-mobile .view-header .view-action:nth-last-child(3) {
  display: none !important;
}


.is-mobile .view-header-nav-buttons,
.is-mobile .view-header .view-action .svg-icon {
  color: orange !important;
  transform: rotate(90deg) scale(0.9);
  --icon-stroke: 1;
  opacity: 0.7;
}
```


