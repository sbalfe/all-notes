# Regular Expressions

- Specify a **pattern** of *text* to *search for*. 
  - Email Addresses
  - Phone numbers
- Regex helps **simplify problems**.

---

- Phone number non regex is specific and limited without adding endless edge cases for various phone number searching.

---

- Regex are descriptions for a pattern of text
  - $\backslash d $  $\to$ stands for **==digit character==** , being any single number from $0 - 9$ 
    - `\d\d\d-\d\d\d-\d\d\d\d` matches the **same text** pattern as *previous* non regex pattern.
- May **become more sophisticated** 
  - Example adding $3$ in braces $(\{3\})$  after **some pattern** $\to$ *match pattern three times* 
  - $\backslash d\{3\}-\backslash d\{3\}-\backslash d\{3\}-\backslash d\{4\}$ - matches same as above.

---

- $\backslash(  \ and  \ \backslash)$ $\to$ in the **raw string** passed to `re.compile()` match the actual parenthesis characters.
  - Following characters have special meanings