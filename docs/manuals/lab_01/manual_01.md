# Lab 1: Learning to use Zensical

This is a sample page to demonstrate the most useful features of Zensical. It is generally quite easy to use. All the normal markdown features are available. 

## Text formatting

**bold text**

*italic text*

***bold and italic***

~~strikethrough~~ [^1]

[^1]: Footnotes can be added to the document like so.

### Code formatting

`inline code`

``` c linenums="25"
// block code (with optional line numbering)
for (int i = 0; i < 10; ++i) {
  printf("Hello World");
}
```

### Lists

Unordered:

- Item 1
- Item 2
    - Nested item

Ordered:

1. First item
2. Second item
3. Third item
    1. Nested item

### Task lists

- [x] Completed task
- [ ] Incomplete task
- [ ] Another task

### Blockquotes

> This is a blockquote
> Multiple lines
>> Nested quote

## Links and images

[Link with title](https://nus.edu.sg "Hover title")

![Alt text](image.jpg "A picture of Neil's best friend")

/// caption

This is a caption. 

///

## Tables

| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Row 1    | Data     | Data     |
| Row 2    | Data     | Data     |


## Horizontal rule

---
or
***
or
___



## Escaping characters

Use backslash to escape: \* \_ \# \`


## Line breaks

End a line with two spaces  
to create a line break.

Or use a blank line for a new paragraph.

## Admonitions

!!! note
    This is a note block.

!!! info
    This is an info block.

!!! question
    This is a question block.

!!! warning
    This is a warning block.

!!! danger
    This is a danger block.

!!! success
    This is a success block. 

!!! tip
    This is a tip block. 

!!! question "You can use custom titles"
    And perhaps ask a question here

??? tip "You can have collapsible blocks too"
    For information most people may not want/need