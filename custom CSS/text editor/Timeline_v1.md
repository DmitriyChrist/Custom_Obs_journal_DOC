---
tags:
  - CSS
---
## What this does?
Очень круто и просто
h2 - выделяет интервал
h3 - описывает лог
h1 - определяет глобально область

Задумка прикольная, но надо попробовать до ума довести (многовато багов с обычной разметкой и списками, шрифты и тд)

## Example:






###  Как использовать
```
> [!timeline]
> ## 2022
> ### Обработка
> С другой стороны, эту систему я всё же развил в другое и более научное направление
>  ## 2021
>  ### Предыдущая мысль
>  вместо details у меня meta и problem.
> # Тут шла глобальная проблема
> ## 2020
> ### методы решения
> Вообще я сейчас посмотрел типичный набор метаданных в своей системе и он выглядит вот так:
> С другой стороны, эту систему я всё же развил в другое и более научное направление – вместо details у меня meta и problem.
> 
> Вообще я сейчас посмотрел типичный набор метаданных в своей системе и он выглядит вот так:
> 


```


## Source
- https://forum.obsidian.md/t/css-snippet-timeline-as-callout/93652

## Code
```css
/*
  TIMELINE as Callout 
*/

.callout[data-callout="timeline"] {
    
  --callout-color: var(--callout-info);
  --callout-icon: clock;

  /* links */
  
  & a {
    color: inherit;
    font-weight: inherit;
    text-decoration: none; 
    font-style: inherit;   
    background: none;
    padding-inline: 0;

    /* hashtags */

    &[href^="#"]{
      text-decoration: none;
      padding-inline: .4rem;
    }

    /* show links on hover */

    &:hover {
      text-decoration: underline solid;
      color: inherit;
    }
  } 
  
  /* content */ 

  & .callout-content {

    background-color: none;
    padding: 2rem 0;

    /* H1 as horizontal line */

    & h1 {
      font-weight: var(--font-normal);
      font-size: var(--font-text-size);
      color: var(--text-normal) !important;
      padding: 0 !important;
      margin-right: 1rem !important;
      text-align: right;
      border-top: dotted .1rem;
    }

    /* H2 as box */
      
    & h2 {
      font-weight: var(--font-normal) !important;
      font-size: var(--font-text-size);
      color: var(--text-muted);
      text-align: left !important;
      margin: 0 1rem;
      padding: .2rem 1rem .2rem 1rem;
      border: solid .1rem;
      border-radius: .4rem;
      display: inline-block;
    }  

   /* vertical line */

    & :is(h1, h3, p, ul, ol) {
      margin: 0 0 0 3.2rem;
      border-left: solid .1rem;
      padding: 0 1rem;
      border-color: var(--text-muted);
      font-weight: var(--font-normal);
      color: var(--text-normal);
      font-size: var(--font-text-size);

      /* space before H1 */      
      &:has(+ h1) {
        padding-bottom: 1rem;
      }        

      /* space before H2 */
      &:has(+ h2) {
        padding-bottom: 1.5rem;
      } 
    }

    /* H3 as bullet */
       
    & h3::before {
        border: .4rem solid;
        border-radius: .4rem;
        border-color: var(--text-muted);
        content: "";
        display: block;
        height: 0;
        left: -1.45rem;
        position: relative;
        top: 1rem;
        width: 0;
        z-index: 1;
    }    

    & h3 {
      color: var(--text-normal);
    }

    /* text */

    & :is(p, ol, ul, li) {
      color: var(--text-faint);
    }

    /* images */
    
    & img {
      padding: 1rem;
    }
  }
}
```

