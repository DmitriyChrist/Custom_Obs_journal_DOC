## Description
This script extends the functionality of the status bar on a mobile device.
- Initial setup:[statusBarAsMain](../../custom%20CSS/user%20interface/statusBarAsMain.md)
This script enables navigating through a tab’s history by swiping up or down on the status bar.
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- typen startup
***

## Instructions


```css

/*Custom status-bar*/
.app-container .status-bar {
  display: flex;
  position: fixed;
  bottom: 23vh;
  background: none !important;
  z-index: var(--layer-cover);
  transform-origin:  right top;
  transform: rotate(90deg);
  border: none;
   /* border: orange 2px solid;*/
}

.app-container .status-bar .svg-icon {
    --icon-stroke: 1;
  display: inline-block;
  transform: rotate(-90deg) scale(1.3);
 /*border: 2px solid #ffb74d;*/
}

/*Improve click-area of Commander icon in status-bar*/
.cmdr.status-bar-item.clickable-icon {
  /*padding-left: 1em;*/
  padding-right: 1.5em;
  /*background: #ffb74d;*/
}

.theme-light .app-container .status-bar .svg-icon {
  color: #000000;
  opacity: 0.4;
}

.theme-dark .app-container .status-bar .svg-icon {
  color: #ffb74d;
  opacity: 0.7;
}

```

```js
export async function invoke(app) {
    if (!app.isMobile) {
        console.log('⚠️ Status Bar Navigation: only for mobile');
        return;
    }

    const statusbar = document.querySelector('.status-bar');
    if (!statusbar) {
        console.warn('⚠️ statusBar not found');
        return;
    }
    
    
    const VERTICAL_THRESHOLD = 80;        
    const HORIZONTAL_TOLERANCE = 30;      
    const MIN_GESTURE_TIME = 50;          
    const MAX_GESTURE_TIME = 800;         
    
    let touchStart = null;
    let isProcessing = false;
    
    const hasBackCommand = app.commands.commands['app:go-back'];
    const hasForwardCommand = app.commands.commands['app:go-forward'];
    
    if (!hasBackCommand || !hasForwardCommand) {
        console.warn('⚠️ command navigation not found');
        return;
    }
    
    function handleTouchStart(e) {
        if (isProcessing) return;
        
        const touch = e.changedTouches[0];
        touchStart = {
            x: touch.clientX,
            y: touch.clientY,
            time: Date.now()
        };
    }
    
    function handleTouchEnd(e) {
        if (!touchStart || isProcessing) return;
        
        const touch = e.changedTouches[0];
        const deltaX = Math.abs(touch.clientX - touchStart.x);
        const deltaY = touch.clientY - touchStart.y;
        const deltaTime = Date.now() - touchStart.time;
        
        touchStart = null;
        
        if (deltaTime < MIN_GESTURE_TIME || deltaTime > MAX_GESTURE_TIME) {
            return;
        }
        
        if (Math.abs(deltaY) < VERTICAL_THRESHOLD) {
            return;
        }
        
        if (deltaX > HORIZONTAL_TOLERANCE) {
            return;
        }
        
        isProcessing = true;
        
        try {
            // History navigation
            if (deltaY > 0) {
                // swipe down = back
                app.commands.executeCommandById('app:go-back');
                
                
            } else {
                // swioe up = forward
                app.commands.executeCommandById('app:go-forward');
                
            }
        } catch (error) {
            console.error('❌ Error history navogation:', error);
        } finally {
            setTimeout(() => {
                isProcessing = false;
            }, 200);
        }
    }
    
    // Добавление event listeners с passive флагом
    statusbar.addEventListener('touchstart', handleTouchStart, { passive: true });
    statusbar.addEventListener('touchend', handleTouchEnd, { passive: true });
    
    console.log('✓ Status Bar Navigation activated');
    console.log('  • swipe down - back to history');
    console.log('  • swipe up - go toward to histoty');
    
    // Cleanup function
    return function() {
        statusbar.removeEventListener('touchstart', handleTouchStart);
        statusbar.removeEventListener('touchend', handleTouchEnd);
        console.log('✓ Status Bar Navigation stopped');
    };
}
```


