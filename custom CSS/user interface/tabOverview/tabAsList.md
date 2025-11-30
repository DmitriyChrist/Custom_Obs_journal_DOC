## Description

CSS snippet that redesigns mobile tab overview into a vertical list format. Features:
- Vertical stacking of tabs
- Compact row layout with icons and controls
- Active tab highlighting with accent border
- Optimized for touch interaction (44px min-height)

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***

![](https://i.imgur.com/d4McmxN.jpeg)
## Code

```css
body.is-mobile {
    
    .mobile-tab-group-container {
        display: flex !important;
        flex-direction: column !important;
        gap: 2px !important;
        margin: 8px !important;
    }
    
    .mobile-tab-wrapper {
        width: 100% !important;
    }
    
    /* === tab as list === */
    .mobile-tab {
        flex-direction: row !important;
        align-items: center !important;
        gap: 10px !important;
        padding: 10px 12px !important;
        background-color: var(--background-primary) !important;
        border: none !important;
        border-left: 3px solid transparent !important;
        border-radius: 4px !important;
        min-height: 44px !important;
    }
    
    .mobile-tab.is-active {
        background-color: var(--background-modifier-active-hover) !important;
        border-left-color: var(--interactive-accent) !important;
    }
    
    /* === icon === */
    .mobile-tab-preview {
        height: 30px;
        width: 30px;
        min-width: 30px !important;
        background-color: transparent !important;
        border-radius: 4px !important;
    }
    
    .mobile-tab-preview:before,
    .mobile-tab-preview-empty,
    .mobile-tab-preview-embed {
        display: none !important;
      
    }
    
    /* === Title === */
    .mobile-tab-title {
        font-size: 1rem;
    }
    
    
    /* === Button === */
    .mobile-tab-pin,
    .mobile-tab .close-button {
        position: relative !important;
        top: auto !important;
        inset-inline-end: auto !important;
        margin-left: auto !important;
        width: 28px !important;
        height: 28px !important;
        background-color: transparent !important;
        --icon-size: 18px !important;
    }
}
.mobile-tab-switcher-menu-button span:first-child {
    font-size: 14px;
    font-weight: 600;
    color: var(--text-accent);
}
```


