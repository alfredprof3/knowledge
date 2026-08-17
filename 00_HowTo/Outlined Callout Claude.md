#type/HowTo #topic/outlined-callout-HowTo #for/all 
# Description
Outlined Callout Claude is a piece of CSS code modified from Kepano's code, which adapts that code to use only a simple syntax for callouts:
`> [!box|info]`
or
`> [!box|bug]`
# Demo
> [!box]+ CSS Code
> Great catch! CSS uses **block comments** `/* ... */` which need both an opening and closing marker — completely different from single-line comment languages. The function needs to be updated to handle this case.
# Usage
## Note
```markdown
> [!box|note]
```
### Output
> [!box|note] Note
> Info callout working with the CSS snippet
## Abstract
```markdown
> [!box|abstract]
```
### Output
> [!box|abstract] Abstract
> Abstract callout working with the CSS snippet
## Info
```markdown
> [!box|info]
```
### Output
> [!box|info] Info
> Info callout working with the CSS snippet
## Todo
```markdown
> [!box|todo]
```
### Output
> [!box|todo] Todo
> Todo callout working with the CSS snippet
## Tip
```markdown
> [!box|tip]
```
### Output
> [!box|tip] Tip
> Tip callout working with the CSS snippet
## Check
```markdown
> [!box|check]
```
### Output
> [!box|check] Check
> Check callout working with the CSS snippet
## Question
```markdown
> [!box|question]
```
### Output
> [!box|question] Question
> Question callout working with the CSS snippet
## Warning
```markdown
> [!box|warning]
```
### Output
> [!box|warning] Warning
> Warning callout working with the CSS snippet
## Failure
```markdown
> [!box|failure]
```
### Output
> [!box|failure] Failure
> Failure callout working with the CSS snippet
## Danger
```markdown
> [!box|danger]
```
### Output
> [!box|danger] Danger
> Danger callout working with the CSS snippet
## Bug
```markdown
> [!box|bug]
```
### Output
> [!box|bug] Bug
> Bug callout working with the CSS snippet
## Example
```markdown
> [!box|example]
```
### Output
> [!box|example] Example
> Example callout working with the CSS snippet
## Quote
```markdown
> [!box|quote]
```
### Output
> [!box|quote] Quote
> Quote callout working with the CSS snippet
# CSS Code

![[00_knowledge/00_CSS-Snippets/Outlined Callout]]