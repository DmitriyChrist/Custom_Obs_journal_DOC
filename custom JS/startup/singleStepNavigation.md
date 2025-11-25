## Description


***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: startup
***




## Code

This is the minimal CSS code required for the script to work correctly.
Before running, make sure that `overflow-x: hidden;`  is set.
```css

body.is-mobile {
    .mobile-tab-group-container {
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        overflow-x: hidden;
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
  const SWIPE_THRESHOLD = 30; // порог свайпа в px
  const ANIMATION_DURATION = 300; // длительность анимации скролла в ms

  const containers = new WeakMap();

  const getWrappers = (container) =>
    Array.from(container.querySelectorAll(TAB_WRAPPER_SELECTOR));

  const clearActive = (container) => {
    container.querySelectorAll('.mobile-tab.is-active').forEach(tab => {
      tab.classList.remove('is-active');
    });
  };

  const scrollWrapperToCenter = (container, wrapper) => {
    const cRect = container.getBoundingClientRect();
    const wRect = wrapper.getBoundingClientRect();

    const containerCenter = cRect.left + cRect.width / 2;
    const wrapperCenter = wRect.left + wRect.width / 2;
    const delta = wrapperCenter - containerCenter;

    container.scrollBy({
      left: delta,
      behavior: 'smooth',
    });
  };

  const selectIndex = (container, index) => {
    const state = containers.get(container);
    if (!state) return;

    // Блокировка при анимации
    if (state.isAnimating) return;

    const wrappers = getWrappers(container);
    if (!wrappers.length) return;

    const clamped = Math.max(0, Math.min(index, wrappers.length - 1));
    const wrapper = wrappers[clamped];
    if (!wrapper) return;

    const activeWrapper = container.querySelector('.mobile-tab.is-active')?.parentElement;
    
    // Если уже активна, не делаем ничего
    if (activeWrapper === wrapper) return;

    state.isAnimating = true;

    clearActive(container);
    wrapper.querySelector('.mobile-tab')?.classList.add('is-active');
    container.dataset.selectedIndex = String(clamped);
    scrollWrapperToCenter(container, wrapper);

    // Разблокировка после анимации
    setTimeout(() => {
      state.isAnimating = false;
    }, ANIMATION_DURATION);
  };

  const getCurrentIndex = (container) => {
    const wrappers = getWrappers(container);
    if (!wrappers.length) return -1;

    const fromData = container.dataset.selectedIndex;
    if (fromData != null) {
      const i = Number(fromData);
      if (!Number.isNaN(i) && i >= 0 && i < wrappers.length) return i;
    }

    const activeWrapper = wrappers.find(w => w.querySelector('.mobile-tab.is-active'));
    if (activeWrapper) return wrappers.indexOf(activeWrapper);

    return 0;
  };

  const setupContainer = (container) => {
    if (!container) return;

    // Удаляем старые слушатели
    if (containers.has(container)) {
      const prev = containers.get(container);
      container.removeEventListener('touchstart', prev.onTouchStart);
      container.removeEventListener('touchend', prev.onTouchEnd);
      container.removeEventListener('click', prev.onClick);
    }

    let startX = 0;
    let startY = 0;
    let startTime = 0;

    const onTouchStart = (e) => {
      const t = e.touches[0];
      startX = t.clientX;
      startY = t.clientY;
      startTime = Date.now();
    };

    const onTouchEnd = (e) => {
      const state = containers.get(container);
      if (!state || state.isAnimating) return;

      const t = e.changedTouches[0];
      const dx = t.clientX - startX;
      const dy = t.clientY - startY;
      const dt = Date.now() - startTime;

      // Игнорируем слишком долгие касания (больше 500ms)
      if (dt > 500) return;

      // Горизонтальный свайп
      if (Math.abs(dx) <= Math.abs(dy) || Math.abs(dx) < SWIPE_THRESHOLD) return;

      const current = getCurrentIndex(container);
      if (current < 0) return;

      let next = current;
      if (dx < 0) {
        // свайп влево → следующая справа
        next = current + 1;
      } else {
        // свайп вправо → предыдущая слева
        next = current - 1;
      }

      selectIndex(container, next);
    };

    const onClick = (evt) => {
      const state = containers.get(container);
      if (!state || state.isAnimating) return;

      const wrapper = evt.target.closest(TAB_WRAPPER_SELECTOR);
      if (!wrapper || !container.contains(wrapper)) return;

      const wrappers = getWrappers(container);
      const idx = wrappers.indexOf(wrapper);
      if (idx === -1) return;

      state.isAnimating = true;

      clearActive(container);
      wrapper.querySelector('.mobile-tab')?.classList.add('is-active');
      container.dataset.selectedIndex = String(idx);
      scrollWrapperToCenter(container, wrapper);

      setTimeout(() => {
        state.isAnimating = false;
      }, ANIMATION_DURATION);
    };

    container.addEventListener('touchstart', onTouchStart, { passive: true });
    container.addEventListener('touchend', onTouchEnd, { passive: true });
    container.addEventListener('click', onClick);

    containers.set(container, {
      onTouchStart,
      onTouchEnd,
      onClick,
      isAnimating: false,
    });

    // Начальное выравнивание без анимации
    const idx = getCurrentIndex(container);
    if (idx >= 0) {
      const wrappers = getWrappers(container);
      const wrapper = wrappers[idx];
      if (wrapper) {
        clearActive(container);
        wrapper.querySelector('.mobile-tab')?.classList.add('is-active');
        container.dataset.selectedIndex = String(idx);
        // Центрируем без анимации при инициализации
        requestAnimationFrame(() => {
          scrollWrapperToCenter(container, wrapper);
        });
      }
    }
  };

  const initAll = () => {
    document.querySelectorAll(CONTAINER_SELECTOR).forEach(container => {
      if (!containers.has(container)) {
        setupContainer(container);
      }
    });
  };

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', initAll, { once: true });
  } else {
    initAll();
  }

  const observer = new MutationObserver((mutations) => {
    let needInit = false;

    for (const m of mutations) {
      // Проверяем только добавление элементов
      for (const node of m.addedNodes) {
        if (node.nodeType !== 1) continue;
        if (node.matches?.(CONTAINER_SELECTOR) || node.querySelector?.(CONTAINER_SELECTOR)) {
          needInit = true;
          break;
        }
      }

      // Очистка удалённых контейнеров
      for (const node of m.removedNodes) {
        if (node.nodeType !== 1) continue;
        if (containers.has(node)) {
          const st = containers.get(node);
          node.removeEventListener('touchstart', st.onTouchStart);
          node.removeEventListener('touchend', st.onTouchEnd);
          node.removeEventListener('click', st.onClick);
          containers.delete(node);
        }
      }
    }

    if (needInit) initAll();
  });

  observer.observe(document.body, { childList: true, subtree: true });
}
```
