Markdown is a lightweight markup language that you can use to add formatting elements to plaintext text documents. Created by John Gruber in 2004, Markdown is now one of the world’s most popular markup languages.

# Headings

- To create a heading, add a the beginning of the title a `#` symbol. The number of `#` (from 1 - 6) determines the size of the heading.

```markdown
# This is Heading 1
## This is Heading 2
### This is Heading 3
#### This is Heading 4
##### This is Heading 5
###### This is Heading 6
```

> [!tip]- Preview: Headings
> # This is Heading 1
> ## This is Heading 2
> ### This is Heading 3
> #### This is Heading 4
> ##### This is Heading 5
> ###### This is Heading 6

## Alternative

- An alternative to insert an `H1` the syntax is as follows.

```markdown
This is a Heading level 1
====
```

> [!tip]- Preview: Alternative Heading 1
> This is a Heading level 1
> ====
> You can add a heading level 1 inserting 4 or more equal symbols.

- To insert an `H2` the syntax is a follows.

```markdown
This is a Heading level 2
----
```

> [!tip]- Preview: Alternative Heading 2
> This is a Heading level 2
> ----
> You can add a heading level 1 inserting 4 or more hyphen symbols.

# Colored Text

- The syntax to color text is as follows.

```markdown
$\color{red}{\textsf{Custom Colored Text}}$
```

> [!tip]- Preview: Colored Text
> $\color{red}{\textsf{Custom Colored Text}}$

# Bold Text

- To add some **Bold** the syntax is pretty simple.

```markdown
**Text in bold**
```

> [!tip]- Preview: Bold Text
> **Text in bold**

# Italics Text

- You can display *italics* text enclosing the word in one pair of `*` asterisks.

```markdown
*This is an italic text*
```

> [!tip]- Preview: Italics Text
> *This is an italic text*

- You can also display _italics_ text enclosing the word in one pair of `_` underscore.

```markdown
_This is a text_
```

> [!tip]- Preview: Italic Text
> _This is an italic text_

# Strikethrough Text

- To strikethrough a text enclose it with `~`

```markdown
~~This a text strikethrough in Markdown syntax~~
```

> [!tip]- Preview: Strikethorugh Text
> ~~This a text strikethrough in Markdown syntax~~

# Highlight Text

- To highlight a text enclose it in double equals symbol <kbd>==</kbd>

```markdown
==Highlighted text in Markdown format==
```

> [!tip]- Preview: Hightlight Text
> ==Highlighted text in Markdown format==

## Alternative

- An alternative to highlight text is using HTML `<mark>`

```html
This is <mark>highlighted text</mark> using HTML.
```

> [!tip]- Preview: Highlight Text HTML
> This is <mark>highlighted text</mark> using HTML.

# Subscript

- To add a subscript text do it with HTML tag.

```html
Water is H<sub>2</sub>O.
```

> [!tip]- Preview: Subscript Text HTML tag
> Water is H<sub>2</sub>O.

# Numbered List

- To display a numbered list just type the number following by a dot.

```markdown
1. Item 1
2. Item 2
3. Item 3
```

> [!tip]- Preview: Numbered List
> 1. Item 1
> 2. Item 2
> 3. Item 3

- Also you can add a sublist numbered.

```markdown
1. Item 1
	1. Subitem 1
	2. Subitem 2
	3. Subitem 3
2. Item 2
3. Item 3
	1. Subitem 1
	2. Subitem 2
4. Item 4
	1. Subitem 1
	2. Subitem 2
	3. Subitem 3
```

> [!tip]- Preview: Numbered List
> 1. Item 1
> 	1. Subitem 1
> 	2. Subitem 2
> 	3. Subitem 3
> 2. Item 2
> 3. Item 3
> 	1. Subitem 1
> 	2. Subitem 2
> 4. Item 4
> 	1. Subitem 1
> 	2. Subitem 2
> 	3. Subitem 3

# Bullet List

- To add a bullet list or unordered list just type a `-` at the beginning of the list.

```markdown
- Item 1
- Item 2
- Item 3
- Item 4
```

> [!tip]- Preview: Bullet List
> - Item 1
> - Item 2
> - Item 3
> - Item 4

- Also you can add subitems.

```markdown
- Item 1
	- Subitem 1
	- Subitem 2
	- Subitem 3
- Item 2
	- Subitem 1
	- Subitem 2
- Item 3
	- Subitem 1
```

> [!tip]- Preview: Bullet List
> - Item 1
> 	- Subitem 1
> 	- Subitem 2
> 	- Subitem 3
> - Item 2
> 	- Subitem 1
> 	- Subitem 2
> - Item 3
> 	- Subitem 1

## Combine Numbered and Bullet Lists

- Ordered list with bullet list as subitems and viceversa.

```markdown
1. Ordered item 1
	- Bullet list subitem 1
	- Bullet list subitem 2
2. Item ordered 2
	1. Subitem ordered 1
	2. Subitem ordered 2
3. Item ordered 3
	1. Bullet list subitem 1

- Bullet item 1
	1. Ordered item 1
	2. Ordered item 2
	3. Ordered item 3
- Bullet item 2
```

> [!tip]- Preview: Combine Ordered and Bullet Lists
> 1. Ordered item 1
> 	- Bullet list subitem 1
> 	- Bullet list subitem 2
> 2. Item ordered 2
> 	1. Subitem ordered 1
> 	2. Subitem ordered 2
> 3. Item ordered 3
> 	1. Bullet list subitem 1
> 
> - Bullet item 1
> 	1. Ordered item 1
> 	2. Ordered item 2
> 	3. Ordered item 3
> - Bullet item 2

# Tasks List (Checkboxes)

- Tasks list starts with a `-` followed by `[ ]` you must add a space inside the brackets.

```markdown
- [ ] Task item number 1
- [ ] Task item number 2
- [x] Task item number 3
```

> [!tip]- Preview: Tasks List
> - [ ] Task item number 1
> - [ ] Task item number 2
> - [x] Task item number 3

# Custom Checkboxes

- In Obsidian some themes include custom checkboxes. To be able and see these checkboxes, install [AnuPpuccin](https://github.com/anubisnekhet/AnuPpuccin), [Things](https://github.com/colineckert/obsidian-things) or [Minimal](https://github.com/kepano/obsidian-minimal) theme.

> [!tip]- Preview: Things Custom Checkboxes
> - [ ] to-do
> - [/] incomplete
> - [x] done
> - [-] canceled
> - [>] forwarded
> - [<] scheduling
> - [?] question
> - [!] important
> - [*] star
> - ["] quote
> - [l] location
> - [b] bookmark
> - [i] information
> - [S] savings
> - [I] idea
> - [p] pros
> - [c] cons
> - [f] fire
> - [k] key
> - [w] win
> - [u] up
> - [d] down
> - [D] draft pull request
> - [P] open pull request
> - [M] merged pull request

> [!tip]- Preview: AnuPpuccin Custom Checkboxes
> - [ ] to-do
> - [/] incomplete
> - [x] done
> - [-] Canceled
> - [>] forwarded
> - [<] scheduling
> - [?] question
> - [!] important
> - [*] star
> - ["] quote
> - [l] location
> - [b] bookmark
> - [i] information
> - [S] saving
> - [I] idea
> - [p] pros
> - [c] cons
> - [f] fire
> - [k] key
> - [w] win
> - [u] up
> - [d] down
> - [0] Speech bubble 0
> - [1] Speech bubble 1
> - [2] Speech bubble 2
> - [3] Speech bubble 3
> - [4] Speech bubble 4
> - [5] Speech bubble 5
> - [6] Speech bubble 6
> - [7] Speech bubble 7
> - [8] Speech bubble 8
> - [9] Speech bubble 9

> [!tip]- Preview: Minimal Custome Checkboxes
> - [ ] to-do
> - [/] incomplete
> - [x] done
> - [-] Canceled
> - [>] forwarded
> - [<] scheduling
> - [?] question
> - [!] important
> - [*] star
> - ["] quote
> - [l] location
> - [b] bookmark
> - [i] information
> - [S] saving
> - [I] idea
> - [p] pros
> - [c] cons
> - [f] fire
> - [k] key
> - [w] win
> - [u] up
> - [d] down

# Inline Code and Code Blocks

You can format code both inline within a sentence, or in its own block.

## Inline code

- Format inline code within a sentence using single \`backticks\`

```markdown
`This is an example of a piece of inline code. This is rendered in monospace font`
```

> [!tip]- Preview: Inline Code
> `This is an example of a piece of inline code. This is rendered in monospace font`

- It is posible to put backticks in an inline code block, surround it with double backticks like so:

```markdown
`` `backtick` ``
```

`` `backtick` ``

## Code Blocks

- To format code as a block, enclose it with four backticks.

`````markdown
````
```
cd ~/Desktop
```
````
`````

> [!tip]- Preview: Code Blocks
> ````
> ```
> cd ~/Desktop
> ```
> ````

## Code Blocks with Syntax Highlighting

- It is possible to use syntax highlighting, just include the language.

````markdown
```javascript
let x = 5;
let y = 6;

let z = x + y;
```
````

> [!tip]- Preview: Code Block Syntax Highlighting
> ```javascript
> let x = 5;
> let y = 6;
> 
> let z = x + y;
> ```

# Plain Text Formatting

- Formatting can be forced to display in plain text by adding a backslash `\` in front of it.

```markdown
\*\*This line will not be bold\*\*
```

> [!tip]- Preview: Plain Text
> \*\*This line will not be bold\*\*

```markdown
\**This line will be italic and show the asterisks*\*
```

> [!tip]- Preview: Plain Text Italics
> \**This line will be italic and show the asterisks*\*

# Links Syntax (External)

- The syntax to insert link in Markdown is as follows.

```markdown
[Custom Text for the Link](https://obsidian.md)
```

> [!tip]- Preview: Link in Markdown Format
> [Custom Text for the Link](https://obsidian.md)

# Internal Links

- To add an internal link, use the same syntax as **External Link** section.

```markdown
[README file](00_knowledge/00_Topics/README.md)
```

> [!tip]- Preview: Internal Link
> [README file](00_knowledge/00_Topics/README.md)

- You can link to a specific heading in the same document or other.

```markdown
[Debian 13 Trixie](00_knowledge/00_Topics/Debian%2013%20Trixie.md#1.%20Mount%20and%20configure%20the%20`/root`%20partition)
```

> [!tip]- Preview: Internal Link to a Heading
> [Debian 13 Trixie](00_knowledge/00_Topics/Debian%2013%20Trixie.md#1.%20Mount%20and%20configure%20the%20`/root`%20partition)

## Alternative

- Another method is using wikilinks.

```markdown
[[00_knowledge/00_Topics/README]]
```

> [!tip]- Preview: Internal Wikilink
> [[00_knowledge/00_Topics/README]]

- Link to a specific heading using wikilinks syntax.

```markdown
[[Markdown syntax#Headings]]
```

> [!tip]- Preview: Internal Wikilink to a Heading
> [[Markdown syntax#Headings]]

- Link to a specific heading and change the link display text

```markdown
[[Markdown syntax#Headings|Markdown Syntax Heading]]
```

> [!tip]- Preview: Internal Wikilink with Custom Name
> [[Markdown syntax#Headings|Markdown Syntax Heading]]

# Reference Links

- You can reference links writing the below or at the bottom of the note.

```markdown
Check out [this article][ref1] and [that guide][ref2].

[ref1]: https://obsidian.md/ "Obsidian"
[ref2]: https://obsidian.md/help/ "Obsidian Help"
```

> [!tip]- Preview: Reference Links
> Check out [this article][ref1] and [that guide][ref2].
> 
> [ref1]: https://obsidian.md/ "Obsidian"
> [ref2]: https://obsidian.md/help/ "Obsidian Help"

# Images

## Absolute path

- To embed images that comes from absolute path (URLs).

```markdown
![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png)
```

> [!tip]- Preview: Image Embed by Absolute Path
> ![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png)

## Relative path

- To embed images that comes from a relative path (Local).

```markdown
![Image in Your Pictures Directory|640](file:///Users/alfredxuser/Downloads/image1.webp)
```

> [!tip]- Preview: Image Embed by Relative Path
> ![Image in Your Pictures Directory|640](file:///Users/alfredxuser/Downloads/image1.webp)
> _⚠️ You need to replace the path to the image you want to embed `Users/UserName/Downloads/image1.webp`_

## Alternative Names

In both ways to embed an image you can add an alternative name for images.

- Enclose it in double quotes `" "`

```markdown
 ![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png "Alt names goes here")
```

> [!tip]- Preview: Image Embed with Alt Name
> ![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png "Alt names goes here")

---

```markdown
![Image in Your Pictures Directory|640](file:///Users/alfredxuser/Downloads/image1.webp)
```

> [!tip]- Preview: Image Embed by Relative Path
> ![Image in Your Pictures Directory|640](file:///Users/alfredxuser/Downloads/image1.webp "Alt names goes here")

_📍 Place the cursor over the image to see a little pop up_

## Linked Images

- Make an image clickable by wrapping it in a link.

```markdown
[![Neon Cyberpunk. Photo by 土豆 地雷 from Pexels](https://images.pexels.com/photos/28122495/pexels-photo-28122495.jpeg)](https://www.pexels.com/photo/cyberpunk-2077-neon-lights-28122495/)
```

> [!tip]- Preview: Linked Image
> [![Neon Cyberpunk. Photo by 土豆 地雷 from Pexels](https://images.pexels.com/photos/28122495/pexels-photo-28122495.jpeg)](https://www.pexels.com/photo/cyberpunk-2077-neon-lights-28122495/)

```markdown
[![Image in Your Pictures Directory|640](file:///Users/alfredxuser/Downloads/image1.webp)](https://obsidian.md)
```

> [!tip]- Preview: Linked Image
> [![Image in Your Pictures Directory|640](file:///Users/alfredxuser/Downloads/image1.webp)](https://obsidian.md)

# Embed




First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

:kissing_heart:

Este es [Video]

[Back to top](#Structured)

[Video]:https://www.youtube.com/watch?v=1_zgKRBrT0Y


### Video embedding

Test for video embedding in markdown file but written with vimwiki

Link and embed video at same time
```md
[![Git and Github](https://img.youtube.com/vi/zZGEuFI9xMY/maxresdefault.jpg)](https://www.youtube.com/watch?v=zZGEuFI9xMY)
```
Example:
[![Git and Github](https://img.youtube.com/vi/zZGEuFI9xMY/maxresdefault.jpg)](https://www.youtube.com/watch?v=zZGEuFI9xMY)

<a href="https://www.youtube.com/embed/zZGEuFI9xMY" target="_blank"><img src="https://img.youtube.com/vi/zZGEuFI9xMY/maxresdefault.jpg" alt="Git and Github" width="400" border="10"></a>

<iframe width="560" height="315" src="https://www.youtube.com/embed/2ReR1YJrNOM" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Types of callouts
>[!note]- Note
>``` 
>>[!note] Note
>>Text goes here
>```

>[!abstract]- Abstract
>``` 
>>[!abstract] Abstract
>>Text goes here
>```

>[!info]- Info
>``` 
>>[!info] Info
>>Text goes here
>```

>[!todo]- Todo
>``` 
>>[!todo] Todo
>>Text goes here
>```

>[!tip]- Tip
>``` 
>>[!tip] Tip
>>Text goes here
>```

>[!check]- succes
>``` 
>>[!check] succes
>>Text goes here
>```

>[!question]- Question
>``` 
>>[!question] Question
>>Text goes here
>```

>[!warning]- Warning
>``` 
>>[!warning] Warning
>>Text goes here
>```

>[!failure]- Failure
>``` 
>>[!failure] Failure
>>Text goes here
>```

>[!danger]- Danger
>``` 
>>[!danger] Danger
>>Text goes here
>```

>[!bug]- Bug
>``` 
>>[!bug] Bug
>>Text goes here
>```

>[!example]- Example
>``` 
>>[!example] Example
>>Text goes here
>```

>[!quote]- Quote
>``` 
>>[!quote] Quote
>>Text goes here
>```

>[!note] Note
>This is a callout inside Obsidian app.
>https://forum.obsidian.md/t/error-153-when-embedding-youtube-videos/105592/21
>[custom callout](https://forum.obsidian.md/t/error-153-when-embedding-youtube-videos/105592/21)

> [!NOTE]+ Level 1  
> note inside level 1
> 
> > [!NOTE]+ Level 2 
> > note inside level 2
>
> > [!NOTE]- Level 2  
> > another note inside level 2
> 
> > note inside level 2
> > Note inside level 3