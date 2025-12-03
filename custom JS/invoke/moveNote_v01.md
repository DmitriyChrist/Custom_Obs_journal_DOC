## Description

This script provides a modal interface in Obsidian that allows the user to move the currently active note to a different subfolder within the same root folder.

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: invoke
***

## Instructions


```
export async function invoke() {
  const app = window.app;
  const vault = app.vault;
  const activeFile = app.workspace.getActiveFile();
  
  if (!activeFile) throw new Error("No active note");

  const pathParts = activeFile.path.split('/');
  if (pathParts.length === 1) throw new Error("Note in the root");
  
  const rootFolder = pathParts[0];
  const rootFolderObj = vault.getAbstractFileByPath(rootFolder);
  
  if (!rootFolderObj?.children) throw new Error("Folder not found: " + rootFolder);
  
  const subFolders = rootFolderObj.children
    .filter(item => item.children)
    .map(folder => folder.name)
    .sort();
  
  if (!subFolders.length) throw new Error("no subFolders in" + rootFolder);

  const currentSubfolder = pathParts[1];
  const { SuggestModal } = require('obsidian');

  class FolderSuggestModal extends SuggestModal {
    constructor(app, folders, current, root, onChoose) {
      super(app);
      this.folders = folders;
      this.current = current;
      this.root = root;
      this.onChoose = onChoose;
      this.setPlaceholder("subFolder in" + root + (current ? " (Now: " + current + ")" : ""));
    }

    getSuggestions(query) {
      return [...this.folders.filter(f => f.toLowerCase().includes(query.toLowerCase())), 
              "[Cancel]"];
    }

    renderSuggestion(folder, el) {
      if (folder === "[Cancel]") {
        el.createEl("div", { text: folder, cls: "mod-muted" });
      } else {
        const text = folder === this.current ? "📁 " + folder + " (current)" : "📁 " + folder;
        el.createEl("div", { text });
      }
    }

    onChooseSuggestion(folder) {
      if (folder !== "[Cancel]") this.onChoose(folder);
    }
  }

  return new Promise((resolve, reject) => {
    const modal = new FolderSuggestModal(app, subFolders, currentSubfolder, rootFolder, 
      async (chosen) => {
        if (chosen === currentSubfolder) return resolve();
        
        const newPath = rootFolder + "/" + chosen + "/" + activeFile.name;
        if (vault.getAbstractFileByPath(newPath)) {
          return reject(new Error("the file already exist"));
        }
        
        await vault.rename(activeFile, newPath);
        const newFile = vault.getAbstractFileByPath(newPath);
        if (newFile) await app.workspace.getLeaf(false).openFile(newFile);
        resolve();
      }
    );
    modal.open();
  });
}
```


