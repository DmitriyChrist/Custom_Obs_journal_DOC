## Description

This script opens the currently active file in Obsidian's right sidebar in source mode. If a leaf (panel) already exists in the right sidebar, it reuses it; otherwise, it creates a new split. The opened file is marked with a custom group ID for future reference, and the sidebar is revealed if it was collapsed. A notification is shown to confirm the action.
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: invoke
***

## Code


```
import { Notice } from "obsidian";

export async function invoke() {
  const GROUP_ID = "cs-right-sidebar";

  // Get the currently active file
  const activeFile = app.workspace.getActiveFile();

  if (!activeFile) {
    new Notice("No active file to open");
    return;
  }

  // Try to get existing leaf in the right sidebar without creating a new split
  let leaf = app.workspace.getRightLeaf(false);

  if (!leaf) {
    // If no leaf exists, create a new split in the right sidebar
    leaf = app.workspace.getRightLeaf(true);
  }

  // Set the view to markdown with the active file
  await leaf.setViewState({
    type: "markdown",
    state: { 
      file: activeFile.path,
      mode: "source" // can be "preview" for reading mode
    },
    active: true
  });

  // Mark this leaf with a custom group ID for later reference
  leaf.setGroup(GROUP_ID);

  // Reveal the leaf if the sidebar is collapsed
  await app.workspace.revealLeaf(leaf);

  new Notice(`Opened "${activeFile.basename}" in right sidebar`);
}
```


