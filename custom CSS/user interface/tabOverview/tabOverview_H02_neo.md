## Description

A futuristic, cyberpunk-themed mobile tab navigation with neon aesthetics and glitch effects.
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
***
![](https://i.imgur.com/YHJorEw.jpeg)

## Code

```css
body.is-mobile {
    /* === Tab container === */
    .mobile-tab-group-container {
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        padding: 2.5em 1em;
        
        overflow-x: auto;
        overflow-y: hidden;
        scroll-snap-type: x mandatory;
        -webkit-overflow-scrolling: touch;
        
        background: #0a0014;
        
        /* Hide scrollbar */
        scrollbar-width: none;
        -ms-overflow-style: none;
    }
    
    .mobile-tab-group-container::-webkit-scrollbar {
        display: none;
    }

    /* === Tab wrapper === */
    .mobile-tab-wrapper {
        flex: 0 0 auto;
        width: 16em;
        scroll-snap-align: start;
    }

    /* === Tab card === */
    .mobile-tab {
        position: relative;
        width: 100%;
        background: linear-gradient(135deg, #1a0033 0%, #0d0020 100%);
        border: 2px solid #ff00ff;
        border-radius: 0; /* Sharp corners */
        
        /* Clipped corners for cyberpunk aesthetic */
        clip-path: polygon(
            0 5px, 5px 0, calc(100% - 5px) 0, 100% 5px,
            100% calc(100% - 5px), calc(100% - 5px) 100%,
            5px 100%, 0 calc(100% - 5px)
        );
        
        opacity: 0.7;
        z-index: 1;
        
        /* Animate specific properties for better performance */
        transition-property: opacity, border-color, transform;
        transition-duration: 0.3s;
        transition-timing-function: ease;
        
        /* Optimize for complex animations */
        will-change: transform;
    }

    /* Glow effect via ::before pseudo-element */
    .mobile-tab::before {
        content: '';
        position: absolute;
        inset: 0;
        border-radius: inherit;
        box-shadow: 
            0 0 20px rgba(255, 0, 255, 0.3),
            0 0 40px rgba(0, 255, 255, 0.2);
        opacity: 1;
        transition: box-shadow 0.3s ease, opacity 0.3s ease;
        pointer-events: none;
        z-index: -1;
    }

    /* === Active state === */
    .mobile-tab.is-active {
        z-index: 10;
        opacity: 1;
        border-color: #00ffff;
        transform: scale(1.08) translateY(-10px);
        
        /* Glitch animation */
        animation: glitch 0.3s infinite;
    }
    
    .mobile-tab.is-active::before {
        box-shadow: 
            0 0 30px rgba(0, 255, 255, 0.6),
            0 0 60px rgba(255, 0, 255, 0.4),
            inset 0 0 30px rgba(0, 255, 255, 0.1);
    }
    
    /* Glitch keyframes animation */
    @keyframes glitch {
        0%, 100% { 
            transform: scale(1.08) translateY(-10px); 
        }
        25% { 
            transform: scale(1.08) translateY(-10px) translateX(2px); 
        }
        50% { 
            transform: scale(1.08) translateY(-10px) translateX(-2px); 
        }
        75% { 
            transform: scale(1.08) translateY(-10px) translateX(1px); 
        }
    }

    /* === Tab preview === */
    .mobile-tab-preview {
        height: 10em;
        position: relative;
        overflow: hidden;
        filter: contrast(1.2) saturate(1.3);
    }
    
    /* Scanlines effect */
    .mobile-tab-preview::after {
        content: '';
        position: absolute;
        inset: 0;
        background: repeating-linear-gradient(
            0deg,
            rgba(0, 255, 255, 0.05) 0px,
            rgba(255, 0, 255, 0.05) 2px,
            transparent 2px,
            transparent 4px
        );
        pointer-events: none;
        animation: scan 8s linear infinite;
    }
    
    @keyframes scan {
        0% { transform: translateY(0); }
        100% { transform: translateY(20px); }
    }

    /* === Tab title === */
    .mobile-tab-title {
        padding: 0.8em 1em;
        font-size: 1.3rem;
        font-weight: 900;
        text-transform: uppercase;
        letter-spacing: 0.1em;
        color: #00ffff;
        text-shadow: 
            0 0 10px rgba(0, 255, 255, 0.8),
            0 0 20px rgba(255, 0, 255, 0.5);
        background: rgba(0, 0, 0, 0.7);
        border-top: 1px solid #ff00ff;
        line-height: 1.3;
    }
    
    .mobile-tab.is-active .mobile-tab-title {
        animation: neon-flicker 1.5s infinite alternate;
    }
    
    @keyframes neon-flicker {
        0%, 100% { opacity: 1; }
        50% { opacity: 0.95; }
    }
    
    /* === Accessibility: Reduced motion === */
    @media (prefers-reduced-motion: reduce) {
        .mobile-tab,
        .mobile-tab::before {
            transition-duration: 0.01ms;
        }
        
        .mobile-tab.is-active {
            animation: none;
            transform: scale(1.08) translateY(-10px);
        }
        
        .mobile-tab-preview::after {
            animation: none;
        }
        
        .mobile-tab.is-active .mobile-tab-title {
            animation: none;
        }
    }
}
```