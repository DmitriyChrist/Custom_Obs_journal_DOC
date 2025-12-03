## Description

This script registers a mobile-only command `toggle-ribbon-menu` for Obsidian that programmatically opens/closes the ribbon menu by simulating a click on the ribbon menu button in the mobile navigation bar 

By creating a registered command, this script enables:
- **Integration with gesture handlers**: The command can be called from swipe gesture scripts to open the ribbon menu with a swipe motion
- **Automation workflows**: Other scripts can programmatically trigger ribbon menu access using `app.commands.executeCommandById('toggle-ribbon-menu')`
- **Custom hotkey assignment**: Users can assign keyboard shortcuts or custom triggers to quickly access the ribbon menu
- **Enhanced mobile navigation**: Particularly useful when combined with custom navigation solutions for faster plugin access


***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: startup
***

## Instructions


```
export async function invoke(app) {
  // Check device
  if (!app.isMobile) {
    console.log('⚠️ Navigation Swipe: for mobile only');
    return;
  }

  app.commands.addCommand({
    id: 'toggle-ribbon-menu',
    name: 'Toggle Ribbon Menu',
    mobileOnly: true,
    callback: () => {
      const ribbonButton = app.mobileNavbar?.ribbonMenuItemEl;
      
      if (!ribbonButton) {
        console.warn('⚠️ Ribbon menu button not found');
        return;
      }
      
      try {
        ribbonButton.click();
      } catch (error) {
        console.error('❌ Click error:', error);
      }
    }
  });
  
  console.log('✓ Navigation Swipe command registred');
}
```


