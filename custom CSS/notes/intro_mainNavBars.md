[Back to => Custom via CSS](../Custom%20via%20CSS.md)

**Introduction**

CSS allows you to modify the appearance of Obsidian interface elements without affecting the functionality of the program or plugins[^1]. Many useful cases can be implemented with styles alone, without additional JavaScript code[^2].

This section contains examples of working with four built-in Obsidian navigation panels. Creating custom navBars using JavaScript will be covered in the "Custom via JS" section.

**Navigation Panels in Obsidian**

Two panels are available by default:
- Top navigation bar (headerNavBar)
![](https://i.imgur.com/rIfJLJw.jpeg)
- Bottom navigation bar (bottomNavBar)
![](https://i.imgur.com/NOKGypX.jpeg)
Two panels are hidden in the mobile version but can be activated via CSS[^3]:
- Status bar (status-bar)
![](https://i.imgur.com/IiEzvxU.jpeg)
- Ribbon panel in the left sidebar
![](https://i.imgur.com/LRKuxv8.jpeg)

**Why Customize the Interface?**

On mobile devices, we interact with the interface through a touchscreen in closer contact than on a computer. Everyone has their own preferences for control element placement, so there is no universal solution. You can configure a setup that works best for you.


**My Configuration Example**

I find it convenient when all buttons are located on the right side and positioned just above the virtual keyboard. It's important that elements don't move or disappear when switching between screens. Therefore, I use this layout:

![](https://i.imgur.com/prwTPnL.jpeg)
- In the upper right corner — headerNavBar
- In the middle right — status-bar
- Using the Commander plugin to add/remove needed commands.

**Why status-bar Instead of bottomNavBar?**

During testing, I discovered a problem with bottomNavBar — the panel disappears when the virtual keyboard appears[^4]. The status-bar proved to be a more stable alternative.

This section will include examples of working with bottomNavBar as well, but keep this behavior quirk in mind[^4][^5]. 

[Back to => Custom via CSS](../Custom%20via%20CSS.md)

***
References:

[^1]: CSS snippets https://help.obsidian.md/snippets

[^2]: How to customize CSS style in Obsidian — setting your own ... https://itwebmind.com/customize-css-style-in-obsidian-app




[^4]: Mobile toolbar and bottom navbar do not show up on keyboard ... https://forum.obsidian.md/t/mobile-toolbar-and-bottom-navbar-do-not-show-up-on-keyboard-activation-and-deactivation-on-older-android-phones-10/104331

[^5]: Android: Navigation bar not shown when using an external keyboard https://forum.obsidian.md/t/android-navigation-bar-not-shown-when-using-an-external-keyboard/94193




