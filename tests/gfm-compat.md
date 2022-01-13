# GitHub Markdown Compatibility #
GitHub-Flavored Markdown has some stuff that Dactyl doesn't do and vice-versa, and various edge cases that the two may interpret differently.

* [GFM Features Not Supported](#gfm-features-not-supported)
* [Line Breaks](#line-breaks)
* [Preprocessing](#preprocessing)
* [Header IDs](#header-ids)
* [Code Blocks](#code-blocks)

## GFM Features Not Supported ##

Dactyl doesn't do any of the following:

* https://github.com/gitlabhq/gitlabhq/blob/master/doc/markdown/markdown.md#url-auto-linking
* [Emoji codes](https://github.com/gitlabhq/gitlabhq/blob/master/doc/markdown/markdown.md#emoji)
* Task lists
    - [x] completed
    - [ ] incomplete
* [GitLab references](https://github.com/gitlabhq/gitlabhq/blob/master/doc/markdown/markdown.md#special-gitlab-references)

**Suggestion:** Avoid using these.

## Line Breaks ##

You can have hard-wrapped paragraphs in Markdown. This
means that not every single-line break introduces a
new paragraph (`<p>` tag). Generally, we don't write
hard-wrapped text for docs, though. (It introduces
unnecessary churn in diffs.)

Two paragraphs should be separated by an empty line.

According to Markdown syntax, you can force a mid-paragraph
line break by ending a line with two spaces  
as demonstrated in this paragraph.

But watch out -- some editors (like [Atom](https://atom.io/docs)) tend to strip off trailing spaces!


## Preprocessing ##

Dactyl does preprocessing with Jinja, so you can do [conditional text](conditionals.html) and [includes](includes.html). GitHub doesn't have stuff like that.

**Suggestion:** For docs that are meant to be viewed on GitHub, use Dactyl's `--md` option. This does pre-processing only, and exports Markdown files suitable for GitHub or elsewhere. Be careful not to overwrite the original files (with the prepropessing directives) with the processed export version.

**Caution:** The pre-processing doesn't know anything about Markdown syntax, so it might catch Jinja-esque syntax in things that weren't meant to be interpreted as such. Use [Jinja's escaping syntax](http://jinja.pocoo.org/docs/dev/templates/#escaping) to fix that. E.g.:

{% raw %}
The {% raw %} block type leaves things inside of it unprocessed.

You can also use the variable delimiter to print a literal, e.g. {{ '{{' }} will come out as {{
{% endraw %}


## Header IDs ##

You can link to headers within a markdown document because each header has a unique ID based on its text. Beware of slight differences across markdown parsers.

**Caution:** Header IDs are case-sensitive and always lower case, for both GitHub-Flavored and Dactyl markdown.

### period. in headers ###
TODO: maybe make this part of `standardize_header_ids` or a new filter

### Emoji in Headers 😀 :smile_cat: ###
GFM converts [emoji codes](http://emoji.codes/) to emoji, then strips them from header IDs. Dactyl doesn't convert, so it only strips "native" emoji from the headers.

**Suggestion:** Avoid emoji.

### underscore_headers ###
Both preserve headers as normal.

A tool used by Ripple before Dactyl converted underscores to dashes in header IDs, so much of the existing documentation links to things based on dashes.

**Suggestion:** Use underscores as normal. For legacy docs, use the standardize_header_ids filter to convert underscores to dashes.

### Asian Characters 漢字など ###

Dactyl strips most such characters from the IDs. GitHub preserves them.

**Suggestion:** Avoid non-ASCII characters in headers.


## Code Blocks ##

You can do code blocks either by the "indented" syntax or with code fences.

Generally, we recommend using code fences. [Code blocks inside lists](lists-and-codeblocks.html) are an exception where you should use indentation to demarcate code blocks.

```
backtick code fence
{
    indentation
    and
    [ "various stuff"](that-might-look-like-markdown-syntax)
}
```

You can put the language name following the first set of backticks to set what language to use for syntax highlighting.
```python
#Python syntax highlighting
if True:
    return "String"
```

**Note:** Use `json` for JSON syntax highlighting.

```no-highlight
this code fence "doesn't" use syntax highlighting (we hope)
```

TODO: improve CSS for "no-highlight" code blocks.

~~~
Don't use tilde-based code fences.
~~~
