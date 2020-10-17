# What is white space 

> Whitespace is any string of text composed only of spaces, tabs or line breaks (to be precise, CRLF sequences, carriage returns or line feeds). 

## Whitespace in html

Whitespaces between words are collapsed into a single character, and whitespace at the start and end of elements and outside elements is ignored.

> This is so that whitespace characters don't impact the layout of your page. Creating space around and inside elements is the job of CSS.

## Whitespace in css

[white-space property](https://developer.mozilla.org/en-US/docs/Web/CSS/white-space)

|           | New lines |  Spaces and tabs  |  Text wrapping  |  End-of-line spaces |
|-----------|-----------|-------------------|-----------------|---------------------|
| normal    | Collapse  | Collapse          | Wrap            | Remove              |
| nowrap    | Collapse  | Collapse          | No wrap         | Remove              |
| pre       | Preserve  | Preserve          | No wrap         | Preserve            |
| pre-wrap  | Preserve  | Preserve          |  Wrap           | Hang                |


## References

* [mdn](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Whitespace)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
