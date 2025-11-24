## Description
We hide the bottomNavBar but do not remove it from the DOM tree.

Note:

Currently, the bottomNavBar hides when the virtual keyboard appears. Interestingly, if you override its position using CSS, it still disappears when the keyboard shows up.

Later, I need to add some old examples, but with this behavior, it’s more of an annoyance in daily use.

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***


## Code

```css
.is-mobile .app-container .mobile-navbar  {
visibility: hidden;
position: fixed;
background: none;
}
```


