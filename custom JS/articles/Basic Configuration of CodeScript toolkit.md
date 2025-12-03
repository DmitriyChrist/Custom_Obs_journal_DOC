
The CodeScript Toolkit plugin allows running scripts from dot-folders. I use this organization structure:

```
.script/
├── startup/
│   ├── modules/
│   │   └── widgets/
│   └── 1_mainStartUp.js
├── lib/ <--for code-button blocks
└── invoke/
```

And the initial plugin settings:
![](https://i.imgur.com/XaJxO0l.jpeg)
This is the fragment of my 1_mainStartUp.js file:
```

export async function invoke(app) {
  console.log('🚀 Start of main modules...');
  
//---------------------------------
//the essential modules
//---------------------------------
  const { invoke: togRib } = await import('./modules/toggleRibbon.js');
  await togRib(app);


//---------------------------------
// Main navigation
//---------------------------------
setTimeout(async () => {
  const { invoke: headerN } = await import('./modules/headerNew.js');
await headerN(app);

  const { invoke: navStB } = await import('./modules/navStatusBar.js');
await navStB(app);

  const { invoke: navSw } = await import('./modules/navigationSwipe.js');
await navSw(app);
}, 100);
```


For more detailed information, I recommend checking:
- [CodeScript Toolkit home page](https://github.com/mnaoumov/obsidian-codescript-toolkit)
- [CodeScript Toolkit documentation](https://github.com/mnaoumov/obsidian-codescript-toolkit/blob/main/docs/usage.md)
- [Demo Vault](https://github.com/mnaoumov/obsidian-codescript-toolkit-demo-vault/)

