## Description

This script provides a function to programmatically switch the active note to preview (reading) mode in Obsidian

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***

## Instructions

Place this code block in your note:
````
```code-button
---
isRaw: true
---
const ctx = codeButtonContext;
const { switchToPreview } = await requireAsync('/lib/previewSwitcher.js');

await switchToPreview(ctx);
```
````

**Setup:**
- Create a `.script/lib` folder in your vault root
- Save the following file as `.script/lib/previewSwitcher.js`:

```js
async function switchToPreview(ctx) {
  const leaf = ctx.app.workspace.activeLeaf;
  
  if (leaf && leaf.view?.getState) {
    const state = leaf.view.getState();
    
    if (state.mode !== 'preview') {
      await leaf.setViewState({
        type: 'markdown',
        state: { ...state, mode: 'preview' }
      });
    }
  }
}

module.exports = { switchToPreview };
```