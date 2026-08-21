## Git Difference Code

```diff
- const old = "remove this line";
+ const updated = "add this line";
  const unchanged = "this stays";
```

---
## Basic Table

```markdown
| Header 1 | Header 2 | Header 3 |
| -------- | -------- | -------- |
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |
```

> [!check]- Result
> 
> | Header 1 | Header 2 | Header 3 |
> | -------- | -------- | -------- |
> | Cell 1   | Cell 2   | Cell 3   |
> | Cell 4   | Cell 5   | Cell 6   |

---
## Advanced Table (Left-align, Center-align, Right-align)

- `:---` or `---` — left-aligned (default)
- `:---:` — center-aligned
- `---:` — right-aligned

```markdown
| Left-aligned | Center-aligned | Right-aligned |
| :----------- | :------------: | ------------: |
| Left         |     Center     |         Right |
| text         |      text      |          text |
```

> [!check]- Result
> 
> | Header 1 | Header 2 | Header 3 |
> | :-------- | :--------: | --------: |
> | Cell 1   | Cell 2   | Cell 3   |
> | Cell 4   | Cell 5   | Cell 6   |

---
