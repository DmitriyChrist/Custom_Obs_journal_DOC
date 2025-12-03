## Description

This script renders a dynamic list of markdown files from a specified folder in Obsidian, filtered by frontmatter properties and sorted by a configurable field

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- initial idea: simple sorted Dataview table
***
## Example

````
```code-button
---
isRaw: true
---
const ctx = codeButtonContext;
const { renderFiles } = await requireAsync('/lib/fileRendererTable.js');

await renderFiles(ctx, {
  folderPath: '+/test',
  filters: { 
    status: 'done'
  },
  displayFields: ['description', 'task']
});
```
````

**Result**:
![](https://i.imgur.com/qGVXls7.jpeg)


## Instruction

### Step 1: Create the executable module
Create a file `.script/lib/fileRenderer.js` and paste the code into it:

```javascript
async function renderFiles(ctx, config) {
  const folderPath = config.folderPath;
  const filters = config.filters || {};
  const displayFields = config.displayFields || ['description', 'task'];
  const sortBy = config.sortBy || null;

  const folder = ctx.app.vault.getAbstractFileByPath(folderPath);
  if (!folder || !folder.children) {
    await ctx.renderMarkdown('*Папка не найдена*');
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
    
    lines.push('- [[' + file.basename + ']]  '); 
    lines.push('*modified:* ' + modified + '  ');
    
    for (let j = 0; j < displayFields.length; j++) {
      const field = displayFields[j];
      const value = frontmatter[field] || '';
      if (field === 'description') {
        lines.push('**' + field + ':** ' + value );
      } else {
        lines.push('**' + field + ':** *' + value + '*  ');
      }
    }
    lines.push('***');
  }

  await ctx.renderMarkdown(lines.length > 0 ? lines.join('\n') : '*Нет файлов*');
}

module.exports = { renderFiles: renderFiles };
```

### Step 2: Use in a note

In any note, create a code-button block:

#### Basic example (filter by status: wip)

````
```code-button
---
isRaw: true
---
const ctx = codeButtonContext;
const { renderFiles } = await requireAsync('/lib/fileRendererTable.js');

await renderFiles(ctx, {
  folderPath: '43_Obsidian_Dev/CodeScript/Scripts',
  filters: { status: 'wip' },
  displayFields: ['description', 'task']
});
```
````

#### Example with sorting
````
```code-button
---
isRaw: true
---
const ctx = codeButtonContext;
const { renderFiles } = await requireAsync('/lib/fileRendererTable.js');

await renderFiles(ctx, {
  folderPath: '43_Obsidian_Dev/CodeScript/Scripts',
  filters: { purpose: 'startup' },
  displayFields: ['description', 'status', 'task'],
  sortBy: 'rank'
});
```
````

#### Example with multiple filters
````

```code-button
---
isRaw: true
---
const ctx = codeButtonContext;
const { renderFiles } = await requireAsync('/lib/fileRendererTable.js');

await renderFiles(ctx, {
  folderPath: '43_Obsidian_Dev/Projects',
  filters: { 
    status: 'active',
    priority: 'high'
  },
  displayFields: ['description', 'deadline', 'assignee']
});
```

````

### Configuration parameters

#### `folderPath` (required)

Path to the folder with files relative to the vault root:
```javascript
folderPath: '43_Obsidian_Dev/CodeScript/Scripts'
```

#### `filters` (optional)

An object with filters by frontmatter fields:

```javascript
filters: { status: 'wip', priority: 'high' }
```

#### `displayFields` (optional)

Array of fields to display:
```javascript
displayFields: ['description', 'task', 'deadline']
```

Default: `['description', 'task']`

#### `sortBy` (optional)

Field to sort by (descending):
```javascript
sortBy: 'rank'  // or 'priority', 'date', etc.
```

#### Frontmatter requirements
Files must have YAML frontmatter with fields for filtering:
```
---
status: wip
purpose: startup
description: Script description
task: Current task
rank: 10
---
```