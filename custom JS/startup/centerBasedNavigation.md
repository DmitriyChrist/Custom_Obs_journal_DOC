## Description


***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: startup
***



## Code

This is the minimal CSS code required for the script to work correctly.
Before running, make sure that `overflow-x: auto;`  is set.

```css

body.is-mobile {
    .mobile-tab-group-container {
        display: flex !important;
        flex-direction: row !important;
        flex-wrap: nowrap !important;
        overflow-x: auto;
        scroll-snap-type: x mandatory;
        -webkit-overflow-scrolling: touch;
    }

    .mobile-tab-wrapper {
        flex-shrink: 0;
        scroll-snap-align: center;
    }
}
```

```js
export async function invoke(app) {
  if (!document.body.classList.contains('is-mobile')) return;

  const CONTAINER_SELECTOR = '.mobile-tab-group-container';
  const TAB_WRAPPER_SELECTOR = '.mobile-tab-wrapper';

  const containers = new WeakMap();
  const scrollTimers = new WeakMap();
  let initTimer = null;

  const clearActive = (container) => {
    if (!container) return;
    container.querySelectorAll('.mobile-tab.is-active').forEach(tab => {
      tab.classList.remove('is-active');
    });
  };

  const selectTab = (wrapper, container) => {
    if (!container || !wrapper) return;
    const activeWrapper = container.querySelector('.mobile-tab.is-active')?.parentElement;
    if (activeWrapper === wrapper) return;

    clearActive(container);
    wrapper.querySelector('.mobile-tab')?.classList.add('is-active');
  };

  const findClosestWrapper = (container) => {
    const containerRect = container.getBoundingClientRect();
    const centerX = containerRect.left + containerRect.width / 2;
    const wrappers = Array.from(container.querySelectorAll(TAB_WRAPPER_SELECTOR));
    if (!wrappers.length) return null;

    let best = null;
    let bestDist = Infinity;

    for (const wrapper of wrappers) {
      const rect = wrapper.getBoundingClientRect();
      const center = rect.left + rect.width / 2;
      const dist = Math.abs(centerX - center);
      if (dist < bestDist) {
        bestDist = dist;
        best = wrapper;
      }
    }
    return best;
  };

  const activateCenteredOnScroll = (container) => {
    const closest = findClosestWrapper(container);
    if (!closest) return;
    selectTab(closest, container);
  };

  const onScroll = (container) => {
    const oldTimer = scrollTimers.get(container);
    if (oldTimer) clearTimeout(oldTimer);

    const timer = setTimeout(() => {
      requestAnimationFrame(() => {
        activateCenteredOnScroll(container);
      });
    }, 40);

    scrollTimers.set(container, timer);
  };

  const onClick = (evt, container) => {
    const wrapper = evt.target.closest(TAB_WRAPPER_SELECTOR);
    if (!wrapper || !container.contains(wrapper)) return;

    clearActive(container);
    wrapper.querySelector('.mobile-tab')?.classList.add('is-active');
  };

  const setupContainer = (container) => {
    if (!container) return;

    // Если уже инициализирован — снимаем старые обработчики
    if (containers.has(container)) {
      const existing = containers.get(container);
      container.removeEventListener('scroll', existing.scrollHandler);
      container.removeEventListener('click', existing.clickHandler);
    }

    const scrollHandler = () => onScroll(container);
    const clickHandler = (evt) => onClick(evt, container);

    container.addEventListener('scroll', scrollHandler, { passive: true });
    container.addEventListener('click', clickHandler, { passive: true });

    containers.set(container, {
      scrollHandler,
      clickHandler,
    });

    // Изначальное выделение
    const wrappers = container.querySelectorAll(TAB_WRAPPER_SELECTOR);
    const activeWrapper = Array.from(wrappers).find(w =>
      w.querySelector('.mobile-tab.is-active')
    );
    if (activeWrapper) {
      selectTab(activeWrapper, container);
    } else {
      // Если активной нет, можно подсветить ближайшую к центру
      const closest = findClosestWrapper(container);
      if (closest) selectTab(closest, container);
    }
  };

  const initAll = () => {
    const all = document.querySelectorAll(CONTAINER_SELECTOR);
    all.forEach(container => {
      if (!containers.has(container)) {
        setupContainer(container);
      }
    });
  };

  const safeInitAll = () => {
    if (initTimer) clearTimeout(initTimer);
    initTimer = setTimeout(initAll, 50);
  };

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', safeInitAll, { once: true });
  } else {
    safeInitAll();
  }

  const mutationObserver = new MutationObserver((mutations) => {
    let needInit = false;

    for (const mutation of mutations) {
      // удалённые контейнеры
      mutation.removedNodes.forEach(node => {
        if (node.nodeType !== 1) return;
        if (containers.has(node)) {
          const state = containers.get(node);
          node.removeEventListener('scroll', state.scrollHandler);
          node.removeEventListener('click', state.clickHandler);
          const t = scrollTimers.get(node);
          if (t) {
            clearTimeout(t);
            scrollTimers.delete(node);
          }
          containers.delete(node);
        }
      });

      // добавленные контейнеры
      mutation.addedNodes.forEach(node => {
        if (node.nodeType !== 1) return;
        if (node.matches?.(CONTAINER_SELECTOR) || node.querySelector?.(CONTAINER_SELECTOR)) {
          needInit = true;
        }
      });
    }

    if (needInit) safeInitAll();
  });

  mutationObserver.observe(document.body, {
    childList: true,
    subtree: true,
  });
}
```
