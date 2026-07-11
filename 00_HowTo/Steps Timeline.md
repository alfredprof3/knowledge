#type/HowTo #topic/steps-timeline #for/all 
# Description
I recently saw on Gemini what an updated version of a step-by-step timeline looked like. I liked it and decided to replicate it by creating, with Claude's help, a CSS snippet that would generate and display my steps in a way similar to a vertical timeline.
# Demo
> [!steps]
> 1. # ⬇️ Download the file
>    
>    _Alternative_
>    
>    Copy the CSS code from here.
>    
> 1. # 📁 Place the file in the CSS snippets folder
>    
>    **Downloaded file**: If you downloaded the file, make sure to name it `Steps-Timeline.css` Move it to `[Your Vault]/.obsidian/snippets/`
>    
>    **Copied CSS code**: If you copy the CSS code directly from the repository, create the CSS file in the same path mentioned above. Use your favorite code editor.
>    
> 1. # 👩🏻‍💻 Open Obsidian app
>    
>    Go to the `Appearance` tab under the ⚙️ icon, scroll down until you see `CSS Snippets` and look for `Steps-Timeline` then, enable it.
>    
> 1. # 📄 Open or Create a new note
>    
>    First type `> [!steps]` to tell Obsidian to use the CSS code, and then start creating the steps as shown in this demo example. Press `Enter` to insert a line break and continue typing.
>    
> 1. # 🔢 Add the order list
>    
>    Create your numbered list in the Markdown format.
# Usage
## First

```markdown
> [!steps]
```
## Add a header title or the name of the step

```markdown
> [!steps]
> 1. This is the first step
```
## Type your description if necessary

```markdown
> [!steps]
> 1. This is the first step
>    Initialize the ordered list and continue adding the steps.
```
## Continue the list

```markdown
> [!steps]
> 1. This is the first step
>    Initialize the ordered list and continue adding the steps.
> 2. This is the second step
>    Continue adding more steps as needed or as necessary to describe your process.
> 3. This is the last step
>    Once you've finished numbering the steps, you'll see the final result. You don't need to add a description.
```
# CSS Code

![[00_knowledge/00_CSS-Snippets/Steps Timeline|Steps Timeline]]