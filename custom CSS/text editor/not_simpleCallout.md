## Description

Callout with background only
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***

![](https://i.imgur.com/i63F55y.jpeg)
![](https://i.imgur.com/VgsJIRU.jpeg)

## Code

```css

/******************************/
/* simple callout */
.theme-dark .callout[data-callout="not"] {
  padding-inline: 0.8rem;
}

.callout[data-callout="not"] .callout-title {
  display: none;
}
  
.callout[data-callout="not"] .callout-content * {
  margin-block: 0.1rem;
}

.theme-dark .callout[data-callout="not"] {
  border-radius: var(--radius-m);
  background: linear-gradient(135deg, 
    rgba(88, 101, 242, 0.15) 0%, 
    rgba(139, 92, 246, 0.15) 50%, 
    rgba(236, 72, 153, 0.15) 100%); 

}

.theme-light .callout[data-callout="not"] {
  border-radius: var(--radius-m);
  box-shadow: var(--shadow-lm-only);
  background: linear-gradient(135deg, 
    rgba(139, 92, 246, 0.08) 0%, 
    rgba(236, 72, 153, 0.08) 50%, 
    rgba(251, 146, 60, 0.08) 100%);
}
```


