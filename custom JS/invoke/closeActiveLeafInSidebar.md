## Description

To use this script as invokable in CodeScript Toolkit
close active note
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: invoke
***

## Code


```
import { Notice } from "obsidian";

export async function invoke() {
  const rightSplit = app.workspace.rightSplit;
  if (!rightSplit) {
    new Notice("Right sidebar not available");
    return;
  }

  // For mobile-drawer use currentTab index
  const currentTabIndex = rightSplit.currentTab;

  if (currentTabIndex === undefined || currentTabIndex === null) {
    new Notice("No active tab in right sidebar");
    return;
  }

  // Get all markdown leaves in the right sidebar
  const allLeaves = app.workspace.getLeavesOfType("markdown");
  
  const rightLeaves = allLeaves.filter((leaf) => {
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

  // Find leaf by currentTab index
  // Since rightSplit.children may contain non-markdown items,
  // we need to match the index with actual leaf
  let activeLeaf = null;
  
  const childAtIndex = rightSplit.children[currentTabIndex];
  
  // Check if this child is among our rightLeaves
  activeLeaf = rightLeaves.find((leaf) => leaf === childAtIndex);

  if (!activeLeaf) {
    // If not found, close the last one
    await rightLeaves[rightLeaves.length - 1].detach();
    new Notice("Closed last note from right sidebar");
    return;
  }

  // Close the active leaf
  await activeLeaf.detach();
  new Notice("Closed active note from right sidebar");
}
```


