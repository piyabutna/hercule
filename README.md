# Hercule

> A simple document transclusion tool ideal for Markdown documents.

[![Build Status](https://travis-ci.org/jamesramsay/hercule.svg)](https://travis-ci.org/jamesramsay/hercule)

```bash
hercule src/document.md -o document.md
```

## Todo

- Coveralls
- NPM dependencies
- Nesting placeholder context

## Writing documents

Writing long documents, particularly technical documents, may require repetition of certain information.
Hercule helps reduce repetition in line with DRY–don't repeat yourself–principles.

```markdown
# Transclusions in Markdown

John Appleseed
(University of Technology)

## Abstract
{{src/abstract.md}}

...

```

Transcluding a document:

```bash
hercule src/transclusions-in-markdown.md -o final.md
```

Omitting the `output` argument allows the output to be piped into other text processing tools:

```bash
hercule src/transclusions-in-markdown.md | pandoc -o final.pdf
```

## Syntax

Example: `report-final.md` is generated by transcluding `apple.md` into `report.md`.

```bash
hercule src/report.md -o report-final.md
```

```markdown
{{apple.md}}
```

Example: `apple.md` and `common/footer.md` are parametrically transcluded into `fruit.md`, which is transcluded into `report.md`.

```bash
hercule src/report.md -o report-final.md
```

```markdown
{{fruit.md fruit:apple.md footer:common/footer.md}}
```

### Simple transclusion

```markdown
An example document

{{DEPENDENCY}}
```

A document may contain any number of transclusions.
Dependencies are parsed, and child dependencies transcluded.
Dependency paths are relative, in the typical `node.js` style.

### Parameterised transclusion

```markdown
An example document

{{DEPENDENCY PLACEHOLDER:DEPENDENCY}}
```

Parameterised transclusions allow a dependency to be specified from the parent file.
The context is only passed to the level immediately below.

### Whitespace sensitivity

Leading whitespace is significant in Markdown.
Hercule preserves whitespace at the beginning of each line.

```markdown
Binary sort example:

  {{snippet.c}}

```

Each line of `snippet.c` will be indented with the whitespace preceding it.

This is also useful for nesting lists or including paragraphs within a list.

```markdown
- Apple
  {{apple-varieties.md}}
- Orange
  {{orange-varieties.md}}
- Pear
  {{pear-varieties.md}}
```
