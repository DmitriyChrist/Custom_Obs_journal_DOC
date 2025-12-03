## Description

Fixes accidental Quick Action Menu appearance during note scrolling on mobile Obsidian.
see also css-styles: [headerNavBarVertical](../../custom%20CSS/user%20interface/headerNavBarVertical.md)
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- initial idea: https://forum.obsidian.md/t/limit-the-mobile-quick-actions-activation-area-to-the-app-header/106512
- author: Abisheik
- type: startup
***

## Instructions

```
export async function invoke(app) {
    if (!app.isMobile) {
        console.log('⚠️ Quick Action Menu Fix: only for mobile');
        return;
    }

    if (window.__swipe_hook_installed) {
        console.log('⚠️ Swipe hook already install');
        return;
    }

    const originalMethods = new Map();

    function hookSwipe(thing, thingName) {
        if (!thing) {
            console.warn(`⚠️ Swipe hook: ${thingName} not found`);
            return false;
        }

        const proto = Object.getPrototypeOf(thing);
        if (!proto || typeof proto.trigger !== "function") {
            console.warn(`⚠️ Swipe hook: ${thingName} dont`t have  trigger`);
            return false;
        }

        if (proto.trigger.__swipe_hooked) {
            console.log(`⚠️ Swipe hook: ${thingName} already hooked`);
            return false;
        }

        const orig = proto.trigger;
        originalMethods.set(proto, orig);

        proto.trigger = function (eventName, data) {
            try {
                if (eventName !== "swipe") {
                    return orig.apply(this, arguments);
                }

                if (!data || data.direction !== "y") {
                    return orig.apply(this, arguments);
                }
                
                const isDown = data.y && data.startY && data.y > data.startY;
                if (!isDown) {
                    return orig.apply(this, arguments);
                }

                const el = data.targetEl || data.touch?.target || data.evt?.target;
                
                if (el?.closest?.('.view-content')) {
                    return;
                }

                return orig.apply(this, arguments);

            } catch (err) {
                console.error('[SwipeHook] Error in trigger override:', err);
                //In case of an error, we allow the standard behavior
                return orig.apply(this, arguments);
            }
        };

        proto.trigger.__swipe_hooked = true;

        console.log(`✓ Swipe hook установлен на: ${thingName}`);
        return true;
    }

    // list objects for hook
    const thingsToHook = [
        { obj: app?.workspace, name: 'workspace' },
        { obj: app?.workspace?.rootSplit, name: 'rootSplit' },
        { obj: app?.workspace?.activeLeaf, name: 'activeLeaf' }
    ];

    let hookedCount = 0;
    for (const { obj, name } of thingsToHook) {
        if (hookSwipe(obj, name)) {
            hookedCount++;
        }
    }

    if (hookedCount === 0) {
        console.warn('⚠️ Couldn't install any swipe hook');
        return;
    }

    window.__swipe_hook_installed = true;

    console.log(`✓ Quick Action Menu Fix activated (${hookedCount} hooks)`);
    console.log('  • Blocks the accidental appearance of the menu when scrolling through a note.');
    console.log('  • Resolves the menu when swiping on header/navbar');

    // Cleanup функция
    return function() {
        console.log('🧹 Recovery origin swipe handlers...');

        let restoredCount = 0;
        for (const [proto, origMethod] of originalMethods.entries()) {
            try {
                proto.trigger = origMethod;
                delete proto.trigger.__swipe_hooked;
                restoredCount++;
            } catch (err) {
                console.warn('[SwipeHook] Error during recovery:', err);
            }
        }

        originalMethods.clear();
        delete window.__swipe_hook_installed;

        console.log(`✓ Quick Action Menu Fix stopped (${restoredCount} restored)`);
    };
}
```