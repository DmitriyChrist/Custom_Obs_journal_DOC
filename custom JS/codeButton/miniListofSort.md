## Description
This is a simplified version of the following: [Frontmatter-Filtered File List](renderListByMeta.md) 
Output features: 
- The description field is displayed inside a `<div>` with a margin-left: 1em indent
- Other fields - ignored

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***
## Example

````
```code-button
---
isRaw: true
---
const ctx = codeButtonContext;
const { renderFiles } = await requireAsync('/lib/fileRendererList.js');

await renderFiles(ctx, {
  folderPath: '+/test',
  filters: { 
    status: 'done'
  }
});
```
````

**Result**:
![](https://i.imgur.com/A4CaHJl.jpeg)

## Code


```js
async function renderFiles(ctx, config) {
  const folderPath = config.folderPath;
  const filters = config.filters || {};
  const displayFields = config.displayFields || ['description', 'task'];
  const sortBy = config.sortBy || null;

  const folder = ctx.app.vault.getAbstractFileByPath(folderPath);
  if (!folder || !folder.children) {
    await ctx.renderMarkdown('*Folder not find*');
    return;
  }

  const files = [];
  const children = folder.children;
  
  for (let i = 0; i < children.length; i++) {
    const child = children[i];
    if (child.children || child.extension !== 'md') continue;
    
    const cache = ctx.app.metadataCache.getFileCache(child);
    const frontmatter = (cache && cache.frontmatter) || {};
    
    let matches = true;
    for (const key in filters) {
      if (frontmatter[key] !== filters[key]) {
        matches = false;
        break;
      }
    }
    
    if (matches) {
      files.push({file: child, frontmatter: frontmatter});
    }
  }

  if (sortBy) {
    files.sort(function(a, b) {
      const aVal = a.frontmatter[sortBy] || 0;
      const bVal = b.frontmatter[sortBy] || 0;
      return bVal - aVal;
    });
  }

  const lines = [];
  
  for (let i = 0; i < files.length; i++) {
    const file = files[i].file;
    const frontmatter = files[i].frontmatter;
    const modified = new Date(file.stat.mtime).toLocaleDateString('ru-RU');
    
    lines.push('- [[' + file.basename + ']]:');
    
    for (let j = 0; j < displayFields.length; j++) {
      const field = displayFields[j];
      const value = frontmatter[field] || '';
      if (field === 'description') {
        lines.push('<div style="margin-left: 1em;">' + value + '</div>');
      }
    }
    
    lines.push('');
  }

  await ctx.renderMarkdown(lines.length > 0 ? lines.join('\n') : '*Not files*');
}

module.exports = { renderFiles: renderFiles };
```


