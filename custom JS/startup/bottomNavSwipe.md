## Description

This script replaces the default mobile navbar in Obsidian with a custom, invisible touch zone at the bottom of the screen that handles gesture-based navigation between tabs. When activated on mobile, it hides the built-in mobile navbar, creates a fixed-height area along the bottom edge, and listens for touchstart and touchend events to detect taps and swipe gestures. A vertical swipe up closes the current tab, while horizontal swipes cycle through tabs in the active tab group, and a short tap temporarily disables the touch zone to let the user interact with underlying UI elements; after a brief delay, the script also exits Vim insert mode in the active editor to keep navigation consistent.

***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- initial idea: https://forum.obsidian.md/t/swipe-the-mobile-navigation-toolbar-to-switch-between-tabs/102969/6?u=dochrist
- author: Abisheik
- type: startup
***

## Instructions
add this css code
```css
/*Custom bottom navbar*/
.is-mobile .app-container .mobile-navbar  {
  visibility: hidden;
  background: none;
  position: fixed;
}
```

```js
export async function invoke(app) {
    if (!app.isMobile) return;

    const mobilenavbar = app.mobileNavbar?.containerEl;
    if (!mobilenavbar) return;
    
    mobilenavbar.style.visibility = 'hidden';
    
    const touchZone = document.createElement('div');
    touchZone.classList.add('mobile-navbar-touch-zone');
    touchZone.style.cssText = `
        position: fixed;
        bottom: 0;
        left: 0;
        right: 0;
        height: 15vh;
        z-index: 100;
        pointer-events: auto;
        //border: 3px solid orange;
        background: none;
    `;
    document.body.appendChild(touchZone);
    
    let touchStartX = 0;
    let touchStartY = 0;
    let touchStartTime = 0;
    let isProcessing = false;
    
    const SWIPE_THRESHOLD = 50;
    const UP_THRESHOLD = window.innerHeight / 5;
    const TAP_TIME_THRESHOLD = 200; 
    const TAP_MOVE_THRESHOLD = 10;  
    
    const exitInsertModeAfterSwipe = (() => {
        let timeoutId = null;
        return () => {
            clearTimeout(timeoutId);
            timeoutId = setTimeout(() => {
                try {
                    const cm = app.workspace?.activeLeaf?.view?.editor?.cm?.cm;
                    if (cm?.state?.vim?.insertMode) {
                        CodeMirror.Vim.exitInsertMode(cm);
                    }
                } catch (err) {}
            }, 150);
        };
    })();
    
    function handleTouchStart(e) {
        if (isProcessing) return;
        
        const touch = e.changedTouches[0];
        touchStartX = touch.clientX;
        touchStartY = touch.clientY;
        touchStartTime = Date.now();
    }
    
    async function handleTouchEnd(e) {
        if (isProcessing) return;
        
        const touch = e.changedTouches[0];
        const touchEndX = touch.clientX;
        const touchEndY = touch.clientY;
        const touchDuration = Date.now() - touchStartTime;
        
        const deltaX = touchEndX - touchStartX;
        const deltaY = touchStartY - touchEndY;
        
        const isTap = (touchDuration < TAP_TIME_THRESHOLD) && 
                      (Math.abs(deltaX) < TAP_MOVE_THRESHOLD) && 
                      (Math.abs(deltaY) < TAP_MOVE_THRESHOLD);
        
        if (isTap) {
    console.log('👆 Tap detected, pull down');
    
    // swich off zone
    touchZone.style.pointerEvents = 'none';
    
    click
    setTimeout(() => {
        touchZone.style.pointerEvents = 'auto';
    }, 100);
    
    return;
}
        // swipeUp - close active tab
        if (deltaY >= UP_THRESHOLD) {
            console.log('⬆️ Swipe up detected');
            const activeLeaf = app.workspace.activeLeaf;
            if (!activeLeaf) return;
            
            isProcessing = true;
            try {
                await activeLeaf.detach();
                exitInsertModeAfterSwipe();
            } catch (error) {
                console.error('❌ Ошибка закрытия вкладки:', error);
            } finally {
                setTimeout(() => { isProcessing = false; }, 200);
            }
            return;
        }
        
        // Проверяем горизонтальный свайп
        if (Math.abs(deltaX) < SWIPE_THRESHOLD) return;
        
        console.log(deltaX < 0 ? '⬅️ Swipe left' : '➡️ Swipe right');
        
        const tabGroup = app.workspace.activeTabGroup;
        if (!tabGroup?.children) return;
        
        const leaves = tabGroup.children;
        if (leaves.length <= 1) return;
        
        const activeLeaf = app.workspace.activeLeaf;
        if (!activeLeaf) return;
        
        const currentIndex = leaves.indexOf(activeLeaf);
        if (currentIndex === -1) return;
        
        let targetIndex = currentIndex;
        
        if (deltaX < 0) {
            targetIndex = (currentIndex + 1) % leaves.length;
        } else {
            targetIndex = (currentIndex - 1 + leaves.length) % leaves.length;
        }
        
        isProcessing = true;
        
        try {
            app.workspace.setActiveLeaf(leaves[targetIndex], { focus: true });
            exitInsertModeAfterSwipe();
        } catch (error) {
            console.error('❌ switch tab error:', error);
        } finally {
            setTimeout(() => { isProcessing = false; }, 200);
        }
    }
    
    touchZone.addEventListener('touchstart', handleTouchStart, { passive: true });
    touchZone.addEventListener('touchend', handleTouchEnd, { passive: true });
    
    console.log('✓ Navigation Swipe activated (100px, border orange)');
    
    return function() {
        touchZone.removeEventListener('touchstart', handleTouchStart);
        touchZone.removeEventListener('touchend', handleTouchEnd);
        document.body.removeChild(touchZone);
        mobilenavbar.style.display = '';
        console.log('✓ Navigation Swipe stopped');
    };
}
```


