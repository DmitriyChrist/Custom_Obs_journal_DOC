## Description

This script creates a dynamic button that displays the count of backlinks to the active note and changes color based on the current Vim mode in the editor. The button appears only when both sidebars are closed, hides otherwise, and executes a configurable command (default: 'toggle-ribbon-menu') on click.
here i used: [Programmatic Ribbon Toggle](toggleRibbon.md) command

**Color indicator:**
- normal: '#50FA7B', // Bright green
- insert: '#FFB86C',        // Orange - for Vim insert mode
- visual: '#F1FA8C',        // Yellow - for Vim visual mode
- 'visual block': '#BD93F9', // Purple/lavender - for Vim visual block mode
- replace: '#FF5555',       // Red - for Vim replace mode
- default: '#888888'        // Gray - default/fallback color

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- initial idea: https://forum.obsidian.md/t/change-the-cursor-color-or-color-the-more-options-button-in-the-obsidian-header-based-on-vim-operating-mode-using-codescript-toolkit/101424
- author: Abisheik
- type: startup
***

## Instructions

### CSS - styles
```
.vim-mode-indicator {
  position: fixed;
  right: 10px;
  z-index: 1000;
  width: 40px;
  height: 40px;
  padding: 0;
  border: none;
  border-radius: 50%;
  color: #000;
  font-size: 18px;
  font-weight: bold;
  user-select: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  pointer-events: auto;
}

.is-mobile .vim-mode-indicator {
  bottom: 51vh;
  right: 1vw;
  width: 25px;
  height: 25px;
  font-size: 12px;
  opacity: 0.4;
  color: black;
}
```

### Executing file:
```js
export async function invoke(app) {
  // =====================
  // CONFIGURATION
  // =====================
  const BUTTON_COMMAND_ID = 'toggle-ribbon-menu'; // Replace with your command Id
  
  let button = null;
  let currentCm = null;
  let cleanup = { timeout: null, listeners: [], interval: null };
  let previousSidebarState = { left: null, right: null };

  const colors = {
    normal: '#50FA7B', insert: '#FFB86C', visual: '#F1FA8C',
    'visual block': '#BD93F9', replace: '#FF5555', default: '#888888'
  };

  function getSidebarState() {
    const leftSplit = app.workspace.leftSplit;
    const rightSplit = app.workspace.rightSplit;
    return {
      left: leftSplit && !leftSplit.collapsed,
      right: rightSplit && !rightSplit.collapsed
    };
  }

  function isSidebarOpen(state) {
    return state.left || state.right;
  }

  function getBacklinksCount() {
  const activeFile = app.workspace.getActiveFile();
  if (!activeFile) return 0;
  
  try {
    const filePath = activeFile.path;
    const resolvedLinks = app.metadataCache.resolvedLinks;
    let count = 0;
    
    // Iterate through all files and check if they link to the current file
    for (const sourcePath in resolvedLinks) {
      const links = resolvedLinks[sourcePath];
      if (links && links[filePath]) {
        count++;
      }
    }
    
    return count;
  } catch (err) {
    console.error('[BacklinkButton] Error fetching backlinks:', err);
    return 0;
  }
}

  function updateButtonDisplay() {
    if (!button) return;
    
    const activeLeaf = app.workspace.activeLeaf;
    if (!activeLeaf || !activeLeaf.view || !activeLeaf.view.editor) return;
    
    const cm = activeLeaf.view.editor.cm;
    if (!cm || !cm.cm || !cm.cm.state || !cm.cm.state.vim) return;
    
    const vimState = cm.cm.state.vim;

    let mode = null;
    if (vimState.visualBlock) {
      mode = 'visual block';
    } else if (vimState.mode) {
      mode = vimState.mode;
    } else if (vimState.insertMode === true) {
      mode = 'insert';
    } else if (vimState.insertMode === false) {
      mode = 'normal';
    }

    // Get Backlinks count
    const backlinksCount = getBacklinksCount();
    
    // Set color based on  Vim mode
    button.style.backgroundColor = mode ? (colors[mode] || colors.normal) : colors.default;
    
    // Display backlink count on button
    button.textContent = String(backlinksCount);
    
    // Update title with mode and backlink info
    const modeText = mode ? mode.charAt(0).toUpperCase() + mode.slice(1) : 'Unknown';
    button.title = `Vim: ${modeText} | Backlinks: ${backlinksCount}`;
  }

  function setButtonVisibility(visible) {
    if (!button) return;
    button.style.display = visible ? 'block' : 'none';
  }

  function updateButton() {
    updateButtonDisplay();

    const currentState = getSidebarState();
    const sidebarOpen = isSidebarOpen(currentState);

    previousSidebarState = currentState;
    setButtonVisibility(!sidebarOpen);
  }

  function checkAndMaybeUpdateSidebarVisibility() {
    const currentState = getSidebarState();
    if (currentState.left !== previousSidebarState.left || 
        currentState.right !== previousSidebarState.right) {
      updateButton();
    }
  }

  function attachVim() {
    const activeLeaf = app.workspace.activeLeaf;
    if (!activeLeaf || !activeLeaf.view || !activeLeaf.view.editor) {
      updateButtonDisplay();
      return;
    }
    
    const cm = activeLeaf.view.editor.cm;
    if (!cm || !cm.cm) {
      updateButtonDisplay();
      return;
    }
    
    const cmInstance = cm.cm;
    
    if (cmInstance === currentCm) {
      updateButtonDisplay();
      return;
    }
    
    if (currentCm) {
      currentCm.off('vim-mode-change', updateButtonDisplay);
    }
    
    cmInstance.on('vim-mode-change', updateButtonDisplay);
    currentCm = cmInstance;
    updateButtonDisplay();
  }

  function initialize() {
    button = document.createElement('button');
    button.className = 'vim-mode-indicator';
    button.textContent = '0';
    button.title = 'Vim: Normal | Обратных ссылок: 0';
    button.style.pointerEvents = 'auto';
    button.style.display = 'none';
    document.body.appendChild(button);

    function handleActiveLeafChange() {
      attachVim();
      updateButton();
    }

    app.workspace.on('active-leaf-change', handleActiveLeafChange);
    cleanup.listeners.push(function() {
      app.workspace.off('active-leaf-change', handleActiveLeafChange);
    });

    // Update on metadata change
    function handleMetadataChange() {
      updateButtonDisplay();
    }
    
    app.metadataCache.on('resolved', handleMetadataChange);
    cleanup.listeners.push(function() {
      app.metadataCache.off('resolved', handleMetadataChange);
    });

    cleanup.interval = setInterval(checkAndMaybeUpdateSidebarVisibility, 700);

    const initialState = getSidebarState();
    previousSidebarState = initialState;
    attachVim();
    updateButton();

    let executing = false;
    function handleClick(e) {
      e.preventDefault();
      e.stopPropagation();
      if (executing || !BUTTON_COMMAND_ID) return;
      executing = true;
      try {
        app.commands.executeCommandById(BUTTON_COMMAND_ID);
      } catch (err) {
        console.error('[BacklinkButton] Error executing command:', err);
      }
      setTimeout(function() { executing = false; }, 150);
    }
    
    button.addEventListener('click', handleClick, { passive: false });
    cleanup.listeners.push(function() {
      button.removeEventListener('click', handleClick);
    });

    app.workspace.onLayoutReady(function() {
      const finalState = getSidebarState();
      previousSidebarState = finalState;
      updateButton();
    });

    cleanup.timeout = setTimeout(function() {
      attachVim();
      const finalState = getSidebarState();
      previousSidebarState = finalState;
      updateButton();
    }, 200);

    console.log('✓ Backlinks indicator activated');
  }

  initialize();

  return function() {
    console.log('🧹 Cleanup in progress...');
    
    if (cleanup.timeout) clearTimeout(cleanup.timeout);
    if (cleanup.interval) clearInterval(cleanup.interval);

    for (let i = 0; i < cleanup.listeners.length; i++) {
      cleanup.listeners[i]();
    }

    if (button) {
      button.remove();
      button = null;
    }

    if (currentCm) {
      currentCm.off('vim-mode-change', updateButtonDisplay);
      currentCm = null;
    }

    console.log('✓ Backlinks indicator stopped');
  };
}
```


