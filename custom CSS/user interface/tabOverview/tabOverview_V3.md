## Description

Wide tab preview tiles:
- Large pin and close icons
- Highlighting and selection of the active tab

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***

![](https://i.imgur.com/szn46k3.jpeg)

## Code

```css


body.is-mobile {
   .mobile-tab-wrapper {
        width: 100%;
    }
    
    /* === Tab as line === */
    .mobile-tab {
      height: 10em;
      padding: 0.5em 1em;
      background-color: var(--background-modifier-active-hover);
      border: 1px solid orange;
      border-radius: 4px;
      opacity: 0.6;
    }
    
    .mobile-tab.is-active {
      border: 1px solid red;
      opacity: 1;
    }
    
    
    /* === Title === */
    .mobile-tab-title {
      padding-bottom: 1rem;
      font-size: 1.5rem;
      border-top: 1px solid orange;
    }
    
    
    /* === Buttons=== */
    .mobile-tab-pin,
    .mobile-tab .close-button {
        width: 3rem;
        height: 3rem;
        --icon-size: 3rem;
    }
}
```


