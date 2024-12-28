TEMPLATE: base.html
TITLE: Sample Markdown File

# Welcome to the Markdown Test

This file is designed to test all **Python-Markdown extensions** in your static site generator. Let's dive in!

---

## 1. Abbreviations (`abbr`)

The HTML standard is often misunderstood.

When writing about HTML, it's good to know that HTML stands for HyperText Markup Language.

*[HTML]: HyperText Markup Language

---

## 2. Attributes (`attr_list`)

Here’s a paragraph with a custom ID and class:

This paragraph has attributes applied. {#custom-id .custom-class}

---

## 3. Definition Lists (`def_list`)

Here’s a definition list:

Term 1  
: This is the definition of Term 1.

Term 2  
: This is the definition of Term 2.

---

## 4. Fenced Code Blocks (`fenced_code`)

```python
def hello_world():
    print("Hello, World!")
```

---

## 5. Footnotes (`footnotes`)

Here’s a sentence with a footnote.[^1]

[^1]: This is the content of the footnote.

---

## 6. Tables (`tables`)

| Name       | Age | Occupation  |
|------------|-----|-------------|
| Alice      | 25  | Developer   |
| Bob        | 30  | Designer    |
| Charlie    | 35  | Manager     |

---

## 7. Smart Emphasis (`smart_strong`)

This text *is* smartly emphasized, and **this is strong**.

---

## 8. Table of Contents (`toc`)

[TOC]

---

## 9. Metadata (`meta`)

Refer to the metadata section at the top of this file.

---

## 10. Admonitions (`admonition`)

!!! note
    This is a note admonition.

!!! warning
    This is a warning admonition.

---

## 11. Code Highlighting (`codehilite`)

```javascript
function greet() {
    console.log("Hello, World!");
}
```

---

## 12. Sane Lists (`sane_lists`)

1. First Item
2. Second Item

---

## 13. WikiLinks (`wikilinks`)

Linking to an internal page: [[HomePage]].

---

## 14. Newline to Break (`nl2br`)

This is the first line.  
And this is the second line.

