## description

The script generates a link in the format `[name](relative/path/to/file.md)`, where the path is specified relative to the base folder, not the vault root.

**Key features:**
- The `BASE_FOLDER` constant defines the root directory from which paths are constructed. In this example, it's `'Custom_Obs_journal_DOC'`.
- Verifies that the file is located inside `BASE_FOLDER`
- Shows a warning if the file is outside the base folder
- Logs attempts of incorrect usage
- URL encoding: Automatically replaces spaces with `%20` for proper link functionality in GitHub and other systems.

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: startup
***

## Instructions

After registering the command via `export async function invoke(app)`, it becomes available through:
- Command Palette (Ctrl/Cmd+P)
- Hotkey (configurable in Settings → Hotkeys)

The script displays notifications about the operation result: successful copying ✓ or errors ❌.


```
// Constant for the base folder — only changed here

const BASE_FOLDER = 'Custom_Obs_journal_DOC';

// Функция для копирования markdown-ссылки с относительным путём
function copyAsMarkdownLink(file) {
    if (!file.path.startsWith(BASE_FOLDER + '/')) {
        new Notice(`⚠️ File is not in the folder ${BASE_FOLDER}!`, 4000);
        console.warn(`⚠️ Attempt to copy link outside the base folder: ${file.path}`);
        return false;
    }
    
    // Removing the base folder from the path
    let relativePath = file.path.substring(BASE_FOLDER.length + 1);
    
    const encodedPath = relativePath.replace(/ /g, '%20');
    const name = file.basename;
    const link = `[${name}](${encodedPath})`;
    
    navigator.clipboard.writeText(link);
    return true;
}

export async function invoke(app) {
    app.commands.addCommand({
        id: 'copy-markdown-link-relative',
        name: 'Copy relative Markdown link (GitHub ready)',
        callback: () => {
            const file = app.workspace.getActiveFile();
            
            if (!file) {
                new Notice('❌ No active file');
                return;
            }
            
            try {
                const success = copyAsMarkdownLink(file);
                if (success) {
                    new Notice('✓ Relative Markdown link copied!');
                }
            } catch (error) {
                console.error('❌ Error copying link:', error);
                new Notice('❌ Error copying link');
            }
        }
    });
    
    console.log(`✓ Copy Relative Markdown Link command registered (base: ${BASE_FOLDER})`);
}
```


