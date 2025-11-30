---
tags:
  - CSS
---
#### Description task
Идея этой реализации взята с темы Cupertino. 
Нечто похожее в [[Разобраться с плавующими кнопками]] (тут нравится прозрачный фон иконки)

Надо найти android-mode с темы и проанализировать
 проблемы теже - по пизде ушёл mobile-navbar.

***

- author: ИИ и мой заспанный мозг
- link:

***

cover::
#### How to

```

```


Варианты решения:
#### Code

```css
.is-mobile .mobile-navbar {
    display: none !important;
}

.mod-root .view-actions button {
  display: flex !important;
  position: fixed !important;
  bottom: 5px;
  width: 52px !important;
  height: 52px !important;
  border-radius: 25px !important;
  box-shadow: 0 3px 10px rgba(0,0,0,0.18);
  background: var(--background-primary-alt, #fff) !important;
  color: var(--text-accent, #443cad) !important;
  justify-content: center;
  align-items: center;
  z-index: 10000 !important;
}

/* Располагаем все подряд слева направо */
.mod-root .view-actions button:nth-child(1) { left: 16px; }
.mod-root .view-actions button:nth-child(2) { left: 80px; }
.mod-root .view-actions button:nth-child(3) { left: 144px; }
.mod-root .view-actions button:nth-child(4) { left: 208px; }
.mod-root .view-actions button:nth-child(5) { left: 272px; }



```






