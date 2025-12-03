## Description

This script is an automatic note relocation module for Obsidian. It tracks changes in the active note and listens for new note creation. When a new note is created, it suggests moving the note into a subfolder of the source note's parent folder using a modal suggestion interface.

This enhances workflow by streamlining note organization based on recent activity and context.

- Problem: By design, it was supposed to trigger only when creating a note from the editor. Now it triggers when creating a new file practically any way.

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: startup
***

## Instructions


```
import { SuggestModal } from 'obsidian';

export async function invoke(app) {
  const vault = app.vault;
  
  // Active files history
  let currentActiveFile = null;
  let previousActiveFile = null;
  
  class FolderSuggestModal extends SuggestModal {
    constructor(app, folders, parentFolder, backlinkFile, onChoose, onCancel) {
      super(app);
      this.folders = folders;
      this.parentFolder = parentFolder;
      this.backlinkFile = backlinkFile;
      this.onChoose = onChoose;
      this.onCancel = onCancel;
      
      const backlinkName = backlinkFile 
        ? backlinkFile.split('/').pop()?.replace('.md', '') || '' 
        : '';
      this.setPlaceholder("Source: " + backlinkName + " → " + parentFolder);
    }

    getSuggestions(query) {
      const filtered = this.folders.filter(folder => 
        folder.toLowerCase().includes(query.toLowerCase())
      );
      
      return [...filtered, "[Leave in current folder]"];
    }

    renderSuggestion(folder, el) {
      if (folder === "[Leave in current folder]") {
        el.createEl("div", { text: folder });
      } else {
        el.createEl("div", { text: "📁 " + folder });
      }
    }

    onChooseSuggestion(folder, evt) {
      if (folder === "[Leave in current folder]") {
        this.onCancel();
      } else {
        this.onChoose(folder);
      }
    }
  }
  
  // Active file change handler
  const activeLeafChangeHandler = () => {
    try {
      const activeFile = app.workspace.getActiveFile();
      
      if (activeFile?.path?.endsWith?.('.md')) {
        // if file changed
        if (currentActiveFile !== activeFile.path) {
          previousActiveFile = currentActiveFile; // saving old
          currentActiveFile = activeFile.path;     // saving new one
          
          console.log('[AutoMove] File changed:', previousActiveFile, '→', currentActiveFile);
        }
      }
    } catch (error) {
      console.error('[AutoMove] Error in activeLeafChangeHandler:', error);
    }
  };
  
  // File creation handler
  const handleCreate = async (file) => {
    try {
      console.log('[AutoMove] === FILE CREATED ===');
      console.log('[AutoMove] File path:', file.path);
      console.log('[AutoMove] Previous active:', previousActiveFile);
      console.log('[AutoMove] Current active:', currentActiveFile);
      
      if (!file?.path?.endsWith?.('.md')) {
        console.log('[AutoMove] Not a markdown file, skipping');
        return;
      }
      
      // A short delay for proper initialization
      await new Promise(resolve => setTimeout(resolve, 500));
      
      // Using the PREVIOUS active file (link source)
      const sourceFile = previousActiveFile;
      
      if (!sourceFile) {
        console.log('[AutoMove] No previous active file');
        return;
      }
      
      console.log('[AutoMove] using as source:', sourceFile);
      
      // Extracting path to parent folder
      const backlinkPathParts = sourceFile.split('/');
      backlinkPathParts.pop();
      const parentFolderPath = backlinkPathParts.join('/');
      console.log('[AutoMove] Parent folder path:', parentFolderPath);
      
      if (!parentFolderPath) {
        console.log('[AutoMove] Parent folder path is empty');
        return;
      }
      
      // Getting folder object
      const parentFolderObj = vault.getAbstractFileByPath(parentFolderPath);
      console.log('[AutoMove] Parent folder object:', !!parentFolderObj);
      
      if (!parentFolderObj?.children) {
        console.log('[AutoMove] Parent folder not found or has no children');
        return;
      }
      
      // Getting subfolders (safely)
      const subFolders = parentFolderObj.children
        .filter(item => item?.children !== undefined)
        .map(folder => folder.name)
        .sort();
      
      console.log('[AutoMove] Subfolders found:', subFolders.length);
      
      if (subFolders.length === 0) {
        console.log('[AutoMove] No subfolders found');
        return;
      }
      
      console.log('[AutoMove] Opening modal...');
      
      return new Promise((resolve) => {
        let resolved = false;
        
        const handleChoice = async (chosenSubfolder) => {
          if (resolved) return;
          resolved = true;
          
          console.log('[AutoMove] Subfolder selected:', chosenSubfolder);
          
          try {
            const fileName = file.name;
            const newPath = parentFolderPath + "/" + chosenSubfolder + "/" + fileName;
            
            console.log('[AutoMove] Move to:', newPath);
            
            // Checking that the file doesn't exist
            const existingFile = vault.getAbstractFileByPath(newPath);
            if (existingFile) {
              console.warn("[AutoMove] Файл уже существует:", newPath);
              resolve();
              return;
            }
            
            // Moving file
            await vault.rename(file, newPath);
            console.log('[AutoMove] File moved successfully');
            
            // Opening the moved file
            const newFile = vault.getAbstractFileByPath(newPath);
            if (newFile) {
              const leaf = app.workspace.getLeaf(false);
              await leaf.openFile(newFile);
              console.log('[AutoMove] File opened');
            }
            
            resolve();
          } catch (error) {
            console.error("[AutoMove] Error during move:", error);
            resolve();
          }
        };
        
        const handleCancel = () => {
          if (resolved) return;
          resolved = true;
          console.log('[AutoMove] Canceling move');
          resolve();
        };
        
        try {
          const modal = new FolderSuggestModal(
            app, 
            subFolders, 
            parentFolderPath, 
            sourceFile,
            handleChoice,
            handleCancel
          );
          
          modal.open();
        } catch (error) {
          console.error('[AutoMove] Error opening modal:', error);
          resolve();
        }
      });
    } catch (error) {
      console.error('[AutoMove] critical error in handleCreate:', error);
    }
  };
  
  // Subscribing to events
  app.workspace.on('active-leaf-change', activeLeafChangeHandler);
  vault.on('create', handleCreate);
  
  console.log('✓ Automatic note relocation module activated');
  console.log('  • Tracks creation of new notes');
  console.log('  • Suggests moving to the source subfolder');
  
  // Cleanup функция
  return function() {
    console.log('[AutoMove] Stopping the module...');
    
    try {
      app.workspace.off('active-leaf-change', activeLeafChangeHandler);
      vault.off('create', handleCreate);
      
      // Clearing state
      currentActiveFile = null;
      previousActiveFile = null;
      
      console.log('✓ Automatic note relocation module stopped');
    } catch (error) {
      console.error('[AutoMove] Error on stop:', error);
    }
  };
}
```


