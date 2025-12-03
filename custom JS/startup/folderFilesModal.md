## Description
This is an advanced file manager for Obsidian that opens a modal window with:
- create the command: `Open Folder Files Modal` in Command pallete
- List of all files in the current folder
- Navigation through subfolders
- Context menus for files and folders
- Recursive viewing (including nested folders)
- Touch gesture support for mobile devices

![](https://i.imgur.com/kO99SWs.jpeg)
![](https://i.imgur.com/SvpEcRM.jpeg)
![](https://i.imgur.com/DQ6JJBL.jpeg)

### 1. Viewing files in the current folder

By default, shows files from the folder containing the currently open note.

### 2. Navigation Mode

**Enable:** Click **"Navigation: OFF"** → becomes **"Navigation: ON"**

**Features:**
- Navigation through subfolders (click folder to open it)
- **⬆️** button to go up one level
- **Breadcrumbs** — path to current folder
- **📋** button to copy current folder path

### 3. Recursive viewing

**Enable:** Click **"Current only"** → becomes **"With subfolders"**

**Features:**
- Shows files from all nested folders
- Displays subfolder path next to files (badge)

### 4. Working with files

- **Click file** — open in current tab
- **Right-click / long press (mobile)** — context menu:

### 5. Working with folders (in navigation mode)

- **Click folder** — enter folder
- **Right-click / long press** — context menu


***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: startup
***


## Code

```js
import { Modal, Notice, Menu } from 'obsidian';

class FolderFilesModal extends Modal {
    constructor(app, folder) {
        super(app);
        this.folder = folder;
        this.currentViewFolder = folder;
        this.files = [];
        this.isRecursive = false;
        this.isNavigationMode = false;
        this.stylesAdded = false;
        this.activeTimers = new Set();
    }
    
    onOpen() {
        if (!this.stylesAdded) {
            this.addStyles();
            this.stylesAdded = true;
        }
        this.renderContent();
    }
    
    renderContent() {
        this.clearAllTimers();
        
        const { contentEl } = this;
        contentEl.empty();
        contentEl.addClass('folder-files-modal');
        
        const header = contentEl.createDiv({ cls: 'modal-title' });
        const titleRow = header.createDiv({ cls: 'title-row' });
        
        if (this.isNavigationMode) {
            const upButton = titleRow.createEl('button', {
                cls: 'nav-up-button',
                title: 'Go up one level'
            });
            upButton.textContent = '⬆️';
            
            upButton.addEventListener('click', () => {
                this.goUp();
            });
            
            const isRoot = !this.currentViewFolder.path || this.currentViewFolder.path === '/';
            if (isRoot) {
                upButton.disabled = true;
                upButton.addClass('disabled');
            }
        }
        
        titleRow.createEl('h2', { text: this.currentViewFolder.name || 'Root' });
        
        if (this.isNavigationMode) {
            const copyPathButton = titleRow.createEl('button', {
                cls: 'copy-path-button',
                title: 'Copy current path'
            });
            copyPathButton.textContent = '📋';
            
            copyPathButton.addEventListener('click', () => {
                this.copyCurrentPath();
            });
        }
        
        if (this.isNavigationMode) {
            this.renderBreadcrumbs(header);
        } else {
            header.createEl('div', { 
                text: this.currentViewFolder.path || 'Root',
                cls: 'mod-muted'
            });
        }
        
        const controlBar = contentEl.createDiv({ cls: 'folder-control-bar' });
        const togglesContainer = controlBar.createDiv({ cls: 'toggles-container' });
        
        const navToggleButton = togglesContainer.createEl('button', {
            cls: 'toggle-button' + (this.isNavigationMode ? ' active' : ''),
            text: this.isNavigationMode ? 'Navigation: ON' : 'Navigation: OFF'
        });
        
        navToggleButton.addEventListener('click', () => {
            this.isNavigationMode = !this.isNavigationMode;
            if (!this.isNavigationMode) {
                this.currentViewFolder = this.folder;
            }
            this.renderContent();
        });
        
        const recursiveToggleButton = togglesContainer.createEl('button', {
            cls: 'toggle-button' + (this.isRecursive ? ' active' : '')
        });
        
        const toggleIcon = recursiveToggleButton.createSpan({ cls: 'toggle-icon' });
        toggleIcon.textContent = this.isRecursive ? '📂' : '📁';
        
        recursiveToggleButton.createSpan({
            text: this.isRecursive ? 'With subfolders' : 'Current only'
        });
        
        recursiveToggleButton.addEventListener('click', () => {
            this.isRecursive = !this.isRecursive;
            this.renderContent();
        });
        
        this.files = this.getAllFiles(this.currentViewFolder, this.isRecursive);
        
        controlBar.createEl('p', {
            text: 'Files: ' + this.files.length,
            cls: 'file-count'
        });
        
        if (this.isNavigationMode) {
            this.renderFoldersList(contentEl);
        }
        
        if (this.files.length === 0) {
            contentEl.createEl('p', { 
                text: 'No markdown files in this folder',
                cls: 'mod-warning'
            });
        } else {
            this.renderFilesList(contentEl);
        }
    }
    
    clearAllTimers() {
        this.activeTimers.forEach(timer => clearTimeout(timer));
        this.activeTimers.clear();
    }
    
    registerTimer(timer) {
        this.activeTimers.add(timer);
    }
    
    unregisterTimer(timer) {
        clearTimeout(timer);
        this.activeTimers.delete(timer);
    }
    
    copyCurrentPath() {
        const path = this.currentViewFolder.path || '/';
        navigator.clipboard.writeText(path).then(() => {
            new Notice('Path copied: ' + path);
        }).catch(() => {
            new Notice('Failed to copy path');
        });
    }
    
    goUp() {
        if (!this.currentViewFolder.parent) {
            return;
        }
        this.currentViewFolder = this.currentViewFolder.parent;
        this.renderContent();
    }
    
    renderBreadcrumbs(container) {
        const breadcrumbsDiv = container.createDiv({ cls: 'breadcrumbs' });
        
        if (!this.currentViewFolder.path || this.currentViewFolder.path === '/') {
            breadcrumbsDiv.createSpan({ 
                text: 'Root',
                cls: 'breadcrumb-item'
            });
            return;
        }
        
        const rootCrumb = breadcrumbsDiv.createSpan({ 
            text: 'Root',
            cls: 'breadcrumb-item clickable'
        });
        
        rootCrumb.addEventListener('click', () => {
            const root = this.app.vault.getRoot();
            this.currentViewFolder = root;
            this.renderContent();
        });
        
        const pathParts = this.currentViewFolder.path.split('/').filter(part => part);
        
        if (pathParts.length > 0) {
            breadcrumbsDiv.createSpan({ text: ' / ', cls: 'breadcrumb-separator' });
        }
        
        pathParts.forEach((part, index) => {
            if (index > 0) {
                breadcrumbsDiv.createSpan({ text: ' / ', cls: 'breadcrumb-separator' });
            }
            
            const breadcrumb = breadcrumbsDiv.createSpan({ 
                text: part,
                cls: 'breadcrumb-item'
            });
            
            if (index < pathParts.length - 1) {
                breadcrumb.addClass('clickable');
                breadcrumb.addEventListener('click', () => {
                    const targetPath = pathParts.slice(0, index + 1).join('/');
                    const targetFolder = this.app.vault.getAbstractFileByPath(targetPath);
                    if (targetFolder && targetFolder.children) {
                        this.currentViewFolder = targetFolder;
                        this.renderContent();
                    }
                });
            }
        });
    }
    
    renderFoldersList(container) {
        const subfolders = this.currentViewFolder.children.filter(
            child => child.children !== undefined
        );
        
        if (subfolders.length === 0) {
            return;
        }
        
        const foldersSection = container.createDiv({ cls: 'folders-section' });
        foldersSection.createEl('h3', { text: 'Folders', cls: 'section-title' });
        
        const foldersList = foldersSection.createDiv({ cls: 'folders-list' });
        
        subfolders.sort((a, b) => a.name.localeCompare(b.name, 'ru'));
        
        subfolders.forEach(subfolder => {
            const folderItem = foldersList.createDiv({ cls: 'folder-item' });
            
            const icon = folderItem.createSpan({ cls: 'folder-icon' });
            icon.textContent = '📁';
            
            const title = folderItem.createSpan({ cls: 'folder-title' });
            title.textContent = subfolder.name;
            
            this.setupFolderItemEvents(folderItem, subfolder);
        });
    }
    
    setupFolderItemEvents(folderItem, folder) {
        let longPressTimer = null;
        let isLongPress = false;
        
        folderItem.addEventListener('click', () => {
            if (!isLongPress) {
                this.currentViewFolder = folder;
                this.renderContent();
            }
            isLongPress = false;
        });
        
        folderItem.addEventListener('contextmenu', (e) => {
            e.preventDefault();
            this.showFolderContextMenu(e, folder);
        });
        
        folderItem.addEventListener('touchstart', (e) => {
            isLongPress = false;
            longPressTimer = setTimeout(() => {
                isLongPress = true;
                const touch = e.touches[0];
                const mouseEvent = new MouseEvent('contextmenu', {
                    bubbles: true,
                    cancelable: true,
                    clientX: touch.clientX,
                    clientY: touch.clientY
                });
                this.showFolderContextMenu(mouseEvent, folder);
                folderItem.addClass('is-long-pressed');
                setTimeout(() => folderItem.removeClass('is-long-pressed'), 300);
            }, 500);
            
            if (longPressTimer) {
                this.registerTimer(longPressTimer);
            }
        }, { passive: true });
        
        folderItem.addEventListener('touchend', () => {
            if (longPressTimer) {
                this.unregisterTimer(longPressTimer);
                longPressTimer = null;
            }
        });
        
        folderItem.addEventListener('touchmove', () => {
            if (longPressTimer) {
                this.unregisterTimer(longPressTimer);
                longPressTimer = null;
                isLongPress = false;
            }
        }, { passive: true });
        
        folderItem.addEventListener('mouseenter', () => {
            folderItem.addClass('is-active');
        });
        
        folderItem.addEventListener('mouseleave', () => {
            folderItem.removeClass('is-active');
        });
    }
    
    showFolderContextMenu(event, folder) {
        const menu = new Menu();
        
        this.app.workspace.trigger('folder-menu', menu, folder, 'file-explorer');
        
        menu.addItem((item) => {
            item
                .setTitle('New note in folder')
                .setIcon('file-plus')
                .onClick(async () => {
                    const newFile = await this.app.fileManager.createNewMarkdownFile(folder);
                    await this.app.workspace.getLeaf(false).openFile(newFile);
                    this.close();
                });
        });
        
        menu.addSeparator();
        
        menu.addItem((item) => {
            item
                .setTitle('Rename folder')
                .setIcon('pencil')
                .onClick(() => {
                    this.app.fileManager.promptForFolderRename(folder);
                });
        });
        
        menu.addItem((item) => {
            item
                .setTitle('Delete folder')
                .setIcon('trash')
                .onClick(() => {
                    this.app.fileManager.promptForFolderDeletion(folder);
                });
        });
        
        menu.addSeparator();
        
        menu.addItem((item) => {
            item
                .setTitle('Copy folder path')
                .setIcon('copy')
                .onClick(() => {
                    navigator.clipboard.writeText(folder.path).then(() => {
                        new Notice('Path copied: ' + folder.path);
                    });
                });
        });
        
        menu.addItem((item) => {
            item
                .setTitle('Reveal in file explorer')
                .setIcon('folder-open')
                .onClick(() => {
                    const firstFile = folder.children.find(child => child.extension);
                    if (firstFile) {
                        this.app.workspace.getLeaf(false).openFile(firstFile).then(() => {
                            this.app.commands.executeCommandById('file-explorer:reveal-file');
                        });
                    }
                    this.close();
                });
        });
        
        menu.showAtMouseEvent(event);
    }
    
    renderFilesList(container) {
        const filesSection = container.createDiv({ cls: 'files-section' });
        
        if (this.isNavigationMode) {
            filesSection.createEl('h3', { text: 'Files', cls: 'section-title' });
        }
        
        const fileList = filesSection.createDiv({ cls: 'folder-files-list' });
        
        const batchSize = 50;
        const renderBatch = (startIndex) => {
            const endIndex = Math.min(startIndex + batchSize, this.files.length);
            
            for (let i = startIndex; i < endIndex; i++) {
                const file = this.files[i];
                const fileItem = fileList.createDiv({ cls: 'folder-file-item' });
                
                const icon = fileItem.createSpan({ cls: 'file-icon' });
                icon.textContent = '📄';
                
                const title = fileItem.createSpan({ cls: 'file-title' });
                title.textContent = file.basename;
                
                if (this.isRecursive && file.parent && file.parent.path !== this.currentViewFolder.path) {
                    const subPath = file.parent.path.replace(this.currentViewFolder.path + '/', '');
                    const badge = fileItem.createSpan({ cls: 'file-badge' });
                    badge.textContent = subPath;
                }
                
                this.setupFileItemEvents(fileItem, file);
            }
            
            if (endIndex < this.files.length) {
                setTimeout(() => renderBatch(endIndex), 0);
            }
        };
        
        renderBatch(0);
    }
    
    setupFileItemEvents(fileItem, file) {
        let longPressTimer = null;
        let isLongPress = false;
        
        fileItem.addEventListener('click', async () => {
            if (!isLongPress) {
                await this.app.workspace.getLeaf(false).openFile(file);
                this.close();
            }
            isLongPress = false;
        });
        
        fileItem.addEventListener('contextmenu', (e) => {
            e.preventDefault();
            this.showFileContextMenu(e, file);
        });
        
        fileItem.addEventListener('touchstart', (e) => {
            isLongPress = false;
            longPressTimer = setTimeout(() => {
                isLongPress = true;
                const touch = e.touches[0];
                const mouseEvent = new MouseEvent('contextmenu', {
                    bubbles: true,
                    cancelable: true,
                    clientX: touch.clientX,
                    clientY: touch.clientY
                });
                this.showFileContextMenu(mouseEvent, file);
                fileItem.addClass('is-long-pressed');
                setTimeout(() => fileItem.removeClass('is-long-pressed'), 300);
            }, 500);
            
            if (longPressTimer) {
                this.registerTimer(longPressTimer);
            }
        }, { passive: true });
        
        fileItem.addEventListener('touchend', () => {
            if (longPressTimer) {
                this.unregisterTimer(longPressTimer);
                longPressTimer = null;
            }
        });
        
        fileItem.addEventListener('touchmove', () => {
            if (longPressTimer) {
                this.unregisterTimer(longPressTimer);
                longPressTimer = null;
                isLongPress = false;
            }
        }, { passive: true });
        
        fileItem.addEventListener('mouseenter', () => {
            fileItem.addClass('is-active');
        });
        
        fileItem.addEventListener('mouseleave', () => {
            fileItem.removeClass('is-active');
        });
    }
    
    showFileContextMenu(event, file) {
        const menu = new Menu();
        
        this.app.workspace.trigger('file-menu', menu, file, 'file-explorer');
        
        menu.addItem((item) => {
            item
                .setTitle('Open in new tab')
                .setIcon('file-plus')
                .onClick(async () => {
                    await this.app.workspace.getLeaf('tab').openFile(file);
                    this.close();
                });
        });
        
        menu.addItem((item) => {
            item
                .setTitle('Open to the right')
                .setIcon('separator-vertical')
                .onClick(async () => {
                    await this.app.workspace.getLeaf('split').openFile(file);
                    this.close();
                });
        });
        
        menu.addSeparator();
        
        menu.addItem((item) => {
            item
                .setTitle('Reveal in file explorer')
                .setIcon('folder-open')
                .onClick(() => {
                    this.app.workspace.getLeaf(false).openFile(file).then(() => {
                        this.app.commands.executeCommandById('file-explorer:reveal-file');
                    });
                    this.close();
                });
        });
        
        menu.showAtMouseEvent(event);
    }
    
    getAllFiles(folder, recursive) {
        const files = [];
        
        const collectFiles = (currentFolder) => {
            currentFolder.children.forEach(child => {
                if (child.extension === 'md') {
                    files.push(child);
                } else if (recursive && child.children) {
                    collectFiles(child);
                }
            });
        };
        
        collectFiles(folder);
        
        return files.sort((a, b) => a.basename.localeCompare(b.basename, 'ru'));
    }
    
    addStyles() {
    const existingStyle = document.getElementById('folder-files-modal-styles');
    if (existingStyle) {
        return;
    }
    
    const styleEl = document.createElement('style');
    styleEl.id = 'folder-files-modal-styles';
    styleEl.textContent = `
        .folder-files-modal .modal-title h2 {
            margin: 0 0 5px 0;
        }
        
        .title-row {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .nav-up-button,
        .copy-path-button {
            padding: 4px 8px;
            background: var(--interactive-normal);
            border: 1px solid var(--background-modifier-border);
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.2s;
            line-height: 1;
        }
        
        .nav-up-button:hover:not(.disabled),
        .copy-path-button:hover {
            background: var(--interactive-hover);
        }
        
        .nav-up-button:active:not(.disabled),
        .copy-path-button:active {
            transform: scale(0.95);
        }
        
        .nav-up-button.disabled {
            opacity: 0.3;
            cursor: not-allowed;
        }
        
        .breadcrumbs {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            margin-top: 5px;
            font-size: 12px;
            color: var(--text-muted);
        }
        
        .breadcrumb-item {
            padding: 2px 4px;
            border-radius: 3px;
        }
        
        .breadcrumb-item.clickable {
            cursor: pointer;
            color: var(--text-accent);
        }
        
        .breadcrumb-item.clickable:hover {
            background: var(--background-modifier-hover);
        }
        
        .breadcrumb-separator {
            margin: 0 4px;
        }
        
        .folder-control-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 15px 0;
            padding: 10px;
            background: var(--background-secondary);
            border-radius: 6px;
            flex-wrap: wrap;
            gap: 10px;
        }
        
        .toggles-container {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
        }
        
        .toggle-button {
            display: flex;
            align-items: center;
            gap: 6px;
            padding: 6px 12px;
            background: var(--interactive-normal);
            border: 1px solid var(--background-modifier-border);
            border-radius: 4px;
            cursor: pointer;
            font-size: 13px;
            transition: all 0.2s;
        }
        
        .toggle-button.active {
            background: var(--interactive-accent);
            color: var(--text-on-accent);
            border-color: var(--interactive-accent);
        }
        
        .toggle-button:hover {
            background: var(--interactive-hover);
        }
        
        .toggle-button.active:hover {
            background: var(--interactive-accent-hover);
        }
        
        .toggle-button:active {
            transform: scale(0.98);
        }
        
        .toggle-icon {
            font-size: 16px;
        }
        
        .file-count {
            margin: 0;
            padding: 6px 12px;
            font-size: 13px;
            color: var(--text-muted);
            background: var(--background-primary);
            border-radius: 4px;
        }
        
        .section-title {
            font-size: 14px;
            font-weight: 600;
            margin: 15px 0 8px 0;
            color: var(--text-normal);
        }
        
        .folders-section {
            margin-bottom: 10px;
        }
        
        .folders-list {
            display: flex;
            flex-direction: column;
            gap: 2px;
        }
        
        .folder-item {
            display: flex;
            align-items: center;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.2s;
            user-select: none;
            -webkit-user-select: none;
            -webkit-tap-highlight-color: transparent;
        }
        
        .folder-item:hover,
        .folder-item.is-active {
            background-color: var(--background-modifier-hover);
        }
        
        .folder-item.is-long-pressed {
            background-color: var(--background-modifier-active-hover);
            transform: scale(0.98);
        }
        
        .folder-icon {
            margin-right: 10px;
            font-size: 16px;
        }
        
        .folder-title {
            flex: 1;
            font-size: 14px;
            font-weight: 500;
        }
        
        .files-section {
            margin-top: 10px;
        }
        
        .folder-files-list {
            max-height: 50vh;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }
        
        .folder-file-item {
            display: flex;
            align-items: center;
            padding: 8px 12px;
            margin: 2px 0;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.2s;
            user-select: none;
            -webkit-user-select: none;
            -webkit-tap-highlight-color: transparent;
        }
        
        .folder-file-item:hover,
        .folder-file-item.is-active {
            background-color: var(--background-modifier-hover);
        }
        
        .folder-file-item.is-long-pressed {
            background-color: var(--background-modifier-active-hover);
            transform: scale(0.98);
        }
        
        .folder-file-item .file-icon {
            margin-right: 10px;
            font-size: 16px;
        }
        
        .folder-file-item .file-title {
            flex: 1;
            font-size: 14px;
        }
        
        .folder-file-item .file-badge {
            font-size: 11px;
            padding: 2px 6px;
            background: var(--background-modifier-border);
            border-radius: 3px;
            margin-left: 10px;
        }
    `;
    document.head.appendChild(styleEl);
}
    
    onClose() {
        this.clearAllTimers();
        const { contentEl } = this;
        contentEl.empty();
    }
}

// ES6 named export
export async function invoke(app) {
    window.FolderFilesModalClass = FolderFilesModal;
    
    window.openFolderModal = () => {
        const file = app.workspace.getActiveFile();
        if (file && file.parent) {
            new window.FolderFilesModalClass(app, file.parent).open();
        } else {
            new Notice('No parent folder');
        }
    };
    
    // registr command in Obsidian
    app.commands.addCommand({
        id: 'open-folder-files-modal',
        name: 'Open Folder Files Modal',
        icon: 'folder-open',
        callback: () => {
            window.openFolderModal();
        }
    });
    
    console.log('✓ module File modal activated');
}
```


