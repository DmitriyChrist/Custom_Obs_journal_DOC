## Description

Generates the first four paragraphs, then cycles through them
***
- author: DOChrist
- link: https://github.com/DmitriyChrist/Custom_Obs_journal_DOC
- type: invoke
***
![](https://i.imgur.com/WbwdTZi.jpeg)
## code

```
import { Modal, Setting, Notice, MarkdownView } from "obsidian";

const loremText = `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent nec dolor vehicula, tristique augue non, convallis nisi. Cras bibendum semper fermentum. Pellentesque cursus augue sed consectetur euismod. Fusce commodo dui ut volutpat egestas. Cras ullamcorper dolor sem, vel euismod quam mollis ut. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc arcu mi, maximus porttitor efficitur ac, ultricies ut augue. Morbi at aliquet nisl. Nunc in consequat leo, nec rutrum nisl. Proin erat lacus, posuere sit amet pulvinar in, fermentum vestibulum nibh. Cras velit ligula, dapibus id elit at, vulputate porttitor purus. Integer dictum ac felis quis sodales. Morbi euismod in orci ac scelerisque. Etiam ultricies tristique velit. Etiam a magna sed metus luctus ullamcorper. Ut malesuada finibus felis ut molestie.

Nulla facilisi. Integer posuere volutpat congue. Duis venenatis leo nulla, semper mollis metus sollicitudin sed. Vestibulum non magna sagittis, volutpat enim at, posuere sapien. Phasellus et libero et enim iaculis dapibus. Vestibulum condimentum metus tortor, at efficitur enim rhoncus ac. Integer eu hendrerit lorem, sit amet imperdiet neque. Sed eleifend risus vel nulla volutpat, ut dictum mi suscipit. Quisque aliquet viverra lorem sed malesuada. Mauris mauris tortor, gravida at malesuada eget, consequat nec risus. Vestibulum odio lorem, luctus sed sem at, sollicitudin vulputate ipsum.

Pellentesque elit nibh, aliquam vel eros eget, ultrices tincidunt orci. Cras at pretium ligula. Vestibulum vitae feugiat ante, in maximus turpis. Morbi nec lacus at erat condimentum varius a mollis magna. Donec quis velit pharetra, semper magna at, ullamcorper libero. In eget enim nisl. Fusce elit lacus, sollicitudin et accumsan non, consequat sit amet quam. Ut quis lacinia risus. Aenean vel mattis nisi.

Phasellus convallis dolor a arcu porta, sit amet tristique diam hendrerit. Aliquam maximus ultrices ipsum, ac facilisis magna aliquam id. Aenean non tortor a risus elementum auctor eu eu orci. Pellentesque tincidunt nibh quis urna bibendum, eget vestibulum diam malesuada. In hac habitasse platea dictumst. Suspendisse potenti. Duis volutpat velit in ex rutrum tristique. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam egestas lectus nec lacus pulvinar interdum. Sed viverra dui facilisis, ullamcorper felis id, faucibus sapien. Mauris pellentesque cursus nunc, gravida aliquam augue sodales non. Praesent rhoncus laoreet felis placerat laoreet. Suspendisse nec sagittis velit. Cras pretium, tortor ut interdum congue, ligula dui venenatis orci, vitae ultrices arcu turpis sit amet ex.`;

class LoremIpsumModal extends Modal {
  constructor(app, onSubmit) {
    super(app);
    this.onSubmit = onSubmit;
    this.type = "words";
    this.amount = 15;
  }

  onOpen() {
    const { contentEl } = this;
    contentEl.createEl("h2", { text: "Lorem Ipsum Generator" });

    new Setting(contentEl)
      .setName("Type")
      .addDropdown(dropdown => dropdown
        .addOption("words", "Words")
        .addOption("sentences", "Sentences")
        .addOption("paragraphs", "Paragraphs")
        .setValue(this.type)
        .onChange(value => this.type = value)
      );

    new Setting(contentEl)
      .setName("Amount")
      .addText(text => text
        .setValue(String(this.amount))
        .onChange(value => {
          const num = parseInt(value);
          if (!isNaN(num) && num > 0) {
            this.amount = num;
          }
        })
      );

    new Setting(contentEl)
      .addButton(btn => btn
        .setButtonText("Generate")
        .setCta()
        .onClick(() => {
          this.close();
          this.onSubmit(this.type, this.amount);
        })
      )
      .addButton(btn => btn
        .setButtonText("Cancel")
        .onClick(() => this.close())
      );
  }

  onClose() {
    this.contentEl.empty();
  }
}

function generateText(type, amount) {

  const paragraphs = loremText.split("\n\n").filter(p => p.trim().length > 0);
  const sentences = loremText.split(/\.\s+/).filter(s => s.trim().length > 0);
  const allWords = loremText.replace(/\n /g, " ").split(" ").filter(w => w.trim().length > 0);
  
  if (type === "words") {
    const result = [];
    for (let i = 0; i < amount; i++) {
      result.push(allWords[i % allWords.length]);
    }
    return result.join(" ");
  }
  
  if (type === "sentences") {
    const result = [];
    for (let i = 0; i < amount; i++) {
      result.push(sentences[i % sentences.length].trim());
    }
    return result.join(". ") + ".";
  }
  
  if (type === "paragraphs") {
    const result = [];
    for (let i = 0; i < amount; i++) {
      result.push(paragraphs[i % paragraphs.length]);
    }
    return result.join("\n");
  }
  
  return "";
}

export async function invoke() {
  new LoremIpsumModal(app, (type, amount) => {
    const activeView = app.workspace.getActiveViewOfType(MarkdownView);
    
    if (!activeView) {
      new Notice("No active note found");
      return;
    }

    const editor = activeView.editor;
    const generatedText = generateText(type, amount);
    editor.replaceSelection(generatedText);
    
    new Notice(`Generated ${amount} ${type}`);
  }).open();
}
```


