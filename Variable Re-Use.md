#templater #input #syntax

To re-use a variable input from `tp.system.prompt("Variable")` use as shown below
# Advanced Method (Variable Re-use)
If you need to use the input multiple times in the same note (e.g., as a title and in the body), use this structure: 

```md
<%*
// Prompt the user
let input = await tp.system.prompt("Enter Title");
%>
# Title: <%* tR += input %>

Here is the body of the note containing: <%* tR += input %>
```
## Key Functions
- **`tp.system.prompt("text")`**: Captures string input.
- **`tR += input`**: Specifically used in execution blocks (`<%* ... %>`) to append the variable `input` to the template result (`tR`).
- **`tp.file.rename(input)`**: Sets the note title to the input.

_Note: Templater executes when you insert the template. It does not dynamically update if you change the input later._
