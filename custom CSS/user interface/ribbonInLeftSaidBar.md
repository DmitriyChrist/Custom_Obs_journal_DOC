## Description

This code displays the hidden ribbon panel in the sidebar. 
The code itself is still very raw, as it uses relative units to fit the panel specifically for my device.

Additionally, the bottomNavBar has the exact same ribbon, but in menu form.
Overall, I'm not sure yet how to use it correctly.

Advantages: the panel is static in the sidebar, even when switching plugins.

As an option, reassign it to something else.
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***

![](attachments/ribbonInLeftorRightSaidBar.jpg)





## Code

### Left side of sidebar
```css
/*left panel*/
.side-dock-ribbon {
  display: block !important;
  pointer-events: auto !important;
  opacity: 1 !important;
  z-index: 1;
  top: 20vh !important;
  left: -2vw !important;
}

/* Hide toggle of sidebare */
.side-dock-ribbon .sidebar-toggle-button.mod-left {
  display: none !important;
}

.nav-files-container {
  padding-left: 35px !important;
}
```

### Right side of sidebar
```css
/*right panel*/
.side-dock-ribbon {
  display: block !important;
  pointer-events: auto !important;
  opacity: 1 !important;
  z-index: 1;
  top: 20vh !important;
  left: 75vw !important;
}

/* Hide toggle of sidebare */
.side-dock-ribbon .sidebar-toggle-button.mod-left {
  display: none !important;
}

.nav-files-container {
  padding-right: 50px !important;
}
```
