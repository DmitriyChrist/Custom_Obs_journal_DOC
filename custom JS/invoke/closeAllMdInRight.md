## Description

To use this script as invokable in CodeScript Toolkit


***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: invoke
***

## Code


```
import { Notice } from "obsidian";

export async function invoke() {
  // Get right sidebar split
  const rightSplit = app.workspace.rightSplit;

  if (!rightSplit) {
    new Notice("Right sidebar not available");
    return;
  }

  // Get all markdown leaves
  const allLeaves = app.workspace.getLeavesOfType("markdown");
  
  // Filter leaves that belong to right sidebar
  const rightLeaves = allLeaves.filter(leaf => {
    let parent = leaf.parent;
    while (parent) {
      if (parent === rightSplit) return true;
      parent = parent.parent;
    }
    return false;
  });

  if (rightLeaves.length === 0) {
    new Notice("No markdown notes in right sidebar");
    return;
  }

  // Detach each leaf (close all notes)
  for (const leaf of rightLeaves) {
    leaf.detach();
  }

  new Notice(`Closed ${rightLeaves.length} note(s)`);
}
```


