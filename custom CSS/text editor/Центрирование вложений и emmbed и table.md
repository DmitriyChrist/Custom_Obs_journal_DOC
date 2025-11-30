## Description

This CSS code centers images, embedded PDFs, videos, tables and iframes in the Markdown preview by making them block elements and setting their left and right margins to auto.

***
- author: 
- link:
***

## Code

```css
/* Centering images */
.markdown-preview-view img {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

/* Centering embedded PDFs, videos, and other iframes */
.markdown-preview-view .markdown-embed,
.markdown-preview-view .internal-embed .markdown-embed-content,
.markdown-preview-view iframe {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

/*Centred table*/
.markdown-preview-view table {
  margin-left: auto;
  margin-right: auto;
}
```