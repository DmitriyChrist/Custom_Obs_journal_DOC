## Description

nice. need test
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***

![](https://i.imgur.com/j5Ozqa2.jpeg)

## Code

```
 /*Золотистый 
color: #fbbf24;
text-shadow: 0 0 5px #fbbf24, 0 0 10px #fbbf24, 0 0 15px #fbbf24;
*/
/* Синий 
color: #60a5fa;
text-shadow: 0 0 5px #60a5fa, 0 0 10px #60a5fa, 0 0 15px #60a5fa;
*/
/* Фиолетовый 
color: #a78bfa;
text-shadow: 0 0 5px #a78bfa, 0 0 10px #a78bfa, 0 0 15px #a78bfa;*/
 
```


```css
.callout[data-callout="note+"] {
  --callout-color: #4ade80;
  position: relative;
  background-color: transparent;
  padding-block-end: clamp(1.5rem, 3vh, 2rem);
  --callout-icon-size: 1.5rem;
  --callout-icon-padding: 1;
  --callout-icon-image: url('data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" width="18" height="18" fill="none" stroke="currentColor" stroke-width="32" stroke-linecap="round" stroke-linejoin="round" %3E%3Cpath d="M172.7 461.6c73.6-149.1 2.1-217-43.7-246.9m72 96.7c71.6-17.3 141-16.3 189.8 88.5m-114-96.3c-69.6-174 44.6-181 16.3-273.6m97.7 370c1.6-3 3.3-5.8 5.1-8.6 20-29.9 34.2-53.2 41.4-65.3a16 16 0 0 0-1.2-17.7 342.1 342.1 0 0 1-40.2-66.1c-10.9-26-12.5-66.5-12.6-86.2 0-7.4-2.4-14.7-7-20.6l-81.8-104a32 32 0 0 0-1.4-1.5m97.7 370a172.8 172.8 0 0 0-18 59c-2.9 21.5-24 38.4-45 32.6-30-8.3-64.5-21.1-95.7-23.5l-47.8-3.6c-7.7-.6-15-4-20.3-9.5l-82.3-84.8c-9-9.2-11.4-23-6.2-34.8l52.8-117.7.7-3M293.1 30a31.5 31.5 0 0 0-44.4-2.3l-97.4 87.5c-5.4 5-9 11.5-10 18.8L129 214.7"/%3E%3C/svg%3E');
}

.callout[data-callout="note+"]::after {  
  content: "";
  /* Иконка */
  display: inline-block;
  position: absolute;
  left: 50%;
  transform: translateX(-50%); 
  bottom: 0;
  width: clamp(1.5rem, 3vw, 2rem);
  height: clamp(1.5rem, 3vh, 2rem);
  background-color: var(--callout-color);
  mask-image: var(--callout-icon-image);
  mask-size: contain;   
  mask-repeat: no-repeat;
}


.callout[data-callout="note+"]::before {
  content: "";
  display: inline-block;
  position: absolute;
  left: 50%;
  bottom: 0.5rem;
  transform: translateX(-50%); 
  width: 70%;
  height: 0.15rem;
 background-image: linear-gradient(
   to right,
   transparent,
   var(--callout-color) calc(50% - var(--callout-icon-padding) * var(--callout-icon-size)),
   transparent calc(50% - var(--callout-icon-padding) * var(--callout-icon-size)),
   transparent calc(50% + var(--callout-icon-padding) * var(--callout-icon-size)),
   var(--callout-color) calc(50% + var(--callout-icon-padding) * var(--callout-icon-size)),
   transparent);
}

.callout[data-callout="note+"] .callout-icon .svg-icon {
  display: none;
}

.callout[data-callout="note+"] .callout-icon::after {
  content: "📁"; 
  scale: 1.2;
  padding-inline-end: 1rem;
  padding-block: 0.5rem;
  color: var(--callout-color);
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

.callout[data-callout="note+"] .callout-fold {
  color: var(--callout-color);
}

.callout[data-callout="note+"] .callout-title-inner {
  position: relative;
  padding-inline-start: 1rem;
  padding-block-end: 0.4rem;
  font-size: 1.2rem;
  color: var(--callout-color);
  border-inline-start: 2px solid var(--callout-color);
}

.callout[data-callout="note+"] .callout-title-inner::before {
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
```


