Markdown is a lightweight markup language that you can use to add formatting elements to plaintext text documents. Created by John Gruber in 2004, Markdown is now one of the world’s most popular markup languages.
# Headings
To create a heading, add up to six `#` symbols before your heading text. The number of `#`symbols determines the size of the heading.

```markdown
# This is Heading 1
## This is Heading 2
### This is Heading 3
#### This is Heading 4
##### This is Heading 5
###### This is Heading 6
```

👇🏻
# This is Heading 1
## This is Heading 2
### This is Heading 3
#### This is Heading 4
##### This is Heading 5
###### This is Heading 6
# Code, code blocks and plain text formatting
You can format code both inline within a sentence, or in its own block.
## Inline code
`This is an example of a piece of inline code. This is rendered in monospace font`

---
You can format code within a sentence using single backticks.
👇🏻
\`backticks\`

---

**Copy and paste** 👇🏻

```markdown
Text inside `backticks` on a line will be formatted like code.
```

Text inside \`backticks\` on a line will be formatted like code.

If you want to put backticks in an inline code block, surround it with double backticks like so:
`` `backtick` ``
##### 11-10-2025, Saturday

---
To format code as a block, enclose it with fout backticks. One more as a code block.

````
```
cd ~/Desktop
```
````


Formatting can be forced to display in plain text by adding a backslash `\` in front of it.

\*\*This line will not be bold\*\*

```markdown
\*\*This line will not be bold\*\*
```

\**This line will be italic and show the asterisks*\*

```markdown
\**This line will be italic and show the asterisks*\*
```




### This is a third-tier heading
You can use one `#` all the way up to `######` six for different heading sizes.

If you'd like to quote someone, use the > character before the line:

> Coffee. The finest organic suspension ever devised... I beat the Borg with it.
> - Captain Janeway

It's very easy to make some words **bold** and other words *italic* with Markdown. You can even [link to Google!](http://google.com)

Sometimes you want numbered lists:

1. One
2. 2. Two
3. 3. Three

Sometimes you want bullet points:

* Start a line with a star
* Profit!

Alternatively,

- Dashes work just as well
- And if you have sub points, put two spaces before the dash or star:
      - Like this
      - And this

If you want to embed images, this is how you do it:

![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png "This is an image")

There are many different ways to style code with GitHub's markdown. If you have inline code blocks, wrap them in backticks: `var example = true`.  If you've got a longer block of code, you can indent with four spaces:

    if (isAwesome){
      return true
    }

GitHub also supports something called code fencing, which allows for multiple lines without indentation:

```
if (isAwesome){
  return true
}
```

And if you'd like to use syntax highlighting, include the language:

```javascript
if (isAwesome){
  return true
}
```

GitHub supports many extras in Markdown that help you reference and link to people. If you ever want to direct a comment at someone, you can prefix their name with an @ symbol: Hey @kneath — love your sweater!

But I have to admit, tasks lists are my favorite:

- [x] This is a complete item
- [ ] This is an incomplete item

When you include a task list in the first comment of an Issue, you will see a helpful progress bar in your list of issues. It works in Pull Requests, too!

And, of course emoji!

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