## Description


***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***
![](https://i.imgur.com/LmcKJr6.jpeg)
![](https://i.imgur.com/UtGhZIg.jpeg)

## Code

```css
/******************************/
/* Callout [!not-] - warm_Custom Neon Style */
/******************************/
.callout[data-callout="not+"] {
  --callout-color: #ff9a56;
  background-color: transparent;
}

.callout[data-callout="not+"] .callout-icon {
  display: none;
}

.callout[data-callout="not+"] .callout-title-inner {
  position: relative;
  margin-right: auto;
  padding-inline-start: 1rem;
  padding-block-end: 0.4rem;
  font-size: 1.2rem;
  color: var(--callout-color);
  border-inline-start: 2px solid var(--callout-color);
}

.callout[data-callout="not+"] .callout-title-inner::before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 70vw;
  height: 2px;
  background: linear-gradient(
    to right,
    var(--callout-color),
    transparent
  );
}

.callout[data-callout="not+"] .callout-fold {
  color: transparent;
}

.callout[data-callout="not+"] .callout-fold::after {
  content: "⌨"; 
  scale: 1.2;
  color: var(--callout-color);
  padding-block: 0.5rem;
  padding-inline-end: 5vw;
  text-shadow: 
    0 0 5px var(--callout-color),
    0 0 10px var(--callout-color),
    0 0 15px var(--callout-color);
  filter: drop-shadow(0 0 3px var(--callout-color));
  animation: neonPulse 2s infinite;
}

@keyframes neonPulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.6; }
}
```


