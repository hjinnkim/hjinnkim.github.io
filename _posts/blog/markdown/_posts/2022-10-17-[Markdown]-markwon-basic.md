---
title: "[Markdown] 01. Basic"
excerpt: "Basic Grammer of Markdown"

categories:
  - Markdown
tags:
  - [markdown]

permalink: /blog/markdown/001/

toc: true
toc_sticky: true
author_profile: true
search: true

date: 2022-10-17
last_modified_at: 2022-10-17
order: 1
published: false
use_math: true

---

# Basic Syntax of Markdown

## 0. Table of Contents

1. [Overview](#1-overview)

## 1. Overview

Markdown is easy to use, manage. If you are a developer, you may see this language in most of Github repositories. You may write down README.md file, the description, manual of your repositories in Markdown. Therefore, Markdown is one of the languages that developers must learn. Also, the Github blog is written in Markdown also. 

Nearly all Markdown applications support the basic syntax outlined in the original Markdown design document. There are minor variations and discrepancies between Markdown processors

Remember that HTML is also useful for using features that are not in Markdown

## 2. Headings

To create a heading, add number signs (#) in front of a word or phrase. Be aware that the number of '#' correponds to the heading level. You need to be careful not to overuse the heading option

<body>
  <table width="100%">
    <tr>
      <td width="30%"><h3>Markdown</h3></td>
      <td width="30%"><h3>HTML</h3></td>
      <td width="40%"><h3>Rendered Output</h3></td>
    </tr>
    <tr>
      <td># Heading level 1</td>
      <td><xmp><h1>Heading level 1</h1></xmp></td>
      <td><h1>Heading level 1</h1></td>
    </tr>
    <tr>
      <td>## Heading level 2</td>
      <td><xmp><h2>Heading level 1</h2></xmp></td>
      <td><h2>Heading level 1</h2></td>
    </tr>
    <tr>
      <td>### Heading level 3</td>
      <td><xmp><h3>Heading level 3</h3></xmp></td>
      <td><h3>Heading level 3</h3></td>
    </tr>
    <tr>
      <td>#### Heading level 4</td>
      <td><xmp><h4>Heading level 4</h4></xmp></td>
      <td><h4>Heading level 4</h4></td>
    </tr>
    <tr>
      <td>##### Heading level 5</td>
      <td><xmp><h5>Heading level 5</h5></xmp></td>
      <td><h5>Heading level 5</h5></td>
    </tr>
  </table>
</body>

### 2.1 Alternate syntax

Alternatively, on the line below the text, add any number of == characters for heading level 1 or -- characters for heading level 2

<body>
  <table width="100%">
    <tr>
      <td width="30%"><h3>Markdown</h3></td>
      <td width="30%"><h3>HTML</h3></td>
      <td width="40%"><h3>Rendered Output</h3></td>
    </tr>
    <tr>
      <td><xmp>Heading level 1
===============</xmp></td>
      <td><xmp><h1>Heading level 1</h1></xmp></td>
      <td><h1>Heading level 1</h1></td>
    </tr>
    <tr>
      <td><xmp>Heading level 2
---------------</xmp></td>
      <td><xmp><h2>Heading level 2</h2></xmp></td>
      <td><h2>Heading level 2</h2></td>
    </tr>
  </table>
</body>

### 2.2 Heading Best Practices

Markdown applications don't agree on how to handle a missing space between the number signs (#) and the heading name. For compatibility, always put a space between the number signs and the heading name.

<body>
    <table width="100%">
      <tr>
        <td width="50%"><h3>✅ Do this</h3></td>
        <td width="50%"><h3>❌ Don't do this</h3></td>
      </tr>
      <tr>
        <td><xmp># Here's a Heading</xmp></td>
        <td><xmp>#Here's a Heading</xmp></td>
      </tr>
    </table>
</body>

You should also put blank lines before and after a heading for compatibility

<style>
  code {
    background-color: #FFFFFF;
    white-space: pre-wrap;
  }
</style>

<body>
<table width="100%">
<tr>
<td width="50%"><h3>✅ Do this</h3></td>
<td width="50%"><h3>❌ Don't do this</h3></td>
</tr>
<tr>
<td>
<code>dffdafasfdsafsafdsafdsafdsfdsafdsfsafsafsafsasafdafsfasf</code>
<div style="display:block">

</div>
<!-- Try to put a blank line before...
<br><br>
# Heading<br><br>
...and after a heading.</> -->
</td>
<td><p>Without blank lines, this might not look right.<br>
# Heading<br>
Don't do this</p></td>
</tr>
</table>
</body>

## 3. Paragraphs

To create paragraphs, use a blank line to separate one or more lines of text

<body>
<table>
<tr>
<td><h3>Markdown</h3></td>
<td><h3>HTML</h3></td>
<td><h3>Rendered Output</h3></td>
</tr>
<tr>
<td width="30%">
<p>I really like using Markdown.</p>
<p>I think I'll use it to format all of my documents from now on.</p>
</td>
<td width="30%">
<p>I really like using Markdown.</p>

<p>I think I'll use it to format all of my documents from now on.</p>
</td>
<td width="40%"><p>I really like using Markdown.</p>

<p>I think I'll use it to format all of my documents from now on.</p></td>
</tr>
<tr>
<td>
  <xmp>
  </xmp>
</td>
<td>
  <xmp>
  </xmp>
</td>
<td>
</td>
</tr>
</table>
</body>


## References

- [Markdown Guide](https://www.markdownguide.org/basic-syntax/)
- [Github docs](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)