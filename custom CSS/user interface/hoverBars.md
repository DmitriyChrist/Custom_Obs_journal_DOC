## Description
This CSS code smoothly changes the opacity of elements inside .status-bar and .view-header when hovered. On hover, the opacity of these elements is set to 1 (fully visible), and when not hovered, it lowers to 0.25 (partially transparent).

***
- author:
- link:
***

## Code

```css

.status-bar:hover .status-bar-item {
    opacity: 1;
    transition: opacity .1s ease-in-out;
}
.status-bar:not(:hover) .status-bar-item {
    opacity: 0.25;
    transition: opacity .25s ease-in-out;
}
.view-header:hover .view-actions {
    opacity: 1;
    transition: opacity .1s ease-in-out;
}
.view-header:not(:hover) .view-actions {
    opacity: 0.25;
    transition: opacity .25s ease-in-out;
}

```


