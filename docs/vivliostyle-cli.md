# Vivliostyle CLI

Vivliostyle CLI is a command line interface for typesetting HTML or Markdown documents.

## Install

You need to install [Node.js](https://nodejs.org/ja/) (v10 or higher, except v14.0.0) prior to using it.

You can install the Vivliostyle CLI with the following command:

```
npm install -g @vivliostyle/cli
```

## Generate PDF from HTML

When an HTML file is specified with the `vivliostyle build` command, a PDF file will be output as a result of typesetting from the HTML.

```
vivliostyle build index.html
```

The default output PDF file name is "output.pdf".

### Specify the output PDF file

The `-o` (`--output`) option can be used to specify a PDF file name.

```
vivliostyle build book.html -o book.pdf
```

### To view the results without outputting PDF

To view the typesetting results without outputting a PDF, see [Preview the typesetting results](#preview-the-typesetting-results).

### Specify the page size

The `-s` (`--size`) option can be used to specify the page size. The possible sizes are A5, A4, A3, B5, B4, JIS-B5, JIS-B4, letter, legal, ledger, or comma-separated width and height.

```
vivliostyle build paper.html -s A4 -o paper.pdf
vivliostyle build letter.html -s letter -o letter.pdf
vivliostyle build slide.html -s 10in,7.5in -o slide.pdf
```

### Specify a Web URL

In addition to a local HTML file, you can also specify a Web URL.

```
vivliostyle build https://vivliostyle.github.io/vivliostyle_doc/samples/gutenberg/Alice.html -s A4 -o Alice.pdf
```

### Specifying single HTML document

The default behavior of Vivliostyle CLI is to typeset multi-document publication (webbook or webpub) if the HTML document specified on the command line contains a table of contents with links to other HTML documents, or if it contains links to a publication manifest. The `-d` (`--single-doc`) option changes this behavior so that only a single HTML document can be typeset.

```
vivliostyle build index.html --single-doc
```

## Specifying additional style sheets

To use a style sheet (CSS file) in addition to the style sheets specified in the HTML file, specify the additional style sheet with `--style` option.

```
vivliostyle build example.html --style additional-style.css
```

The style sheet specified in this way is treated as [author style sheet](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade#author_stylesheets), as if it is specified in the HTML file at very last, and can override styles of other style sheets according to the CSS cascading rules.

### Specifying user style sheets

To use a [user style sheet](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade#user_stylesheets), specify the style sheet with `--user-style` option. (User style sheet cannot
override styles of author style sheets unless specifying `!important`).

```
vivliostyle build example.html --user-style user-style.css
```

## Generate PDF from EPUB

When an EPUB file is specified with the `vivliostyle build` command, a PDF file will be output as a result of typesetting from the EPUB.

```
vivliostyle build epub-sample.epub -s A5 --user-style epub-style.css -o epub-sample.pdf
```

### Example of user style sheet for EPUB

To typeset EPUB with your preferred page style, you need to [specify a user style sheet](#specifying-user-style-sheets).

Example of user style sheet for EPUB: epub-style.css
```css
@page {
  margin: 10%;
  @top-center {     /* page header */
    writing-mode: horizontal-tb;
    font-size: 75%;
    content: string(title);
  }
  @bottom-center {  /* page footer */
    writing-mode: horizontal-tb;
    font-size: 67%;
    content: counter(page);
  }
}
@page :first {      /* cover page */
  margin: 0;
  @top-center {
    content: none;
  }
  @bottom-center {
    content: none;
  }
}
title {
  string-set: title content();
}
img { /* to fit images in the page */
  max-width: 100vw !important;
  max-height: 100vh !important;
}
```

### Generate PDF from unzipped EPUB

To generate a PDF from an unzipped EPUB, specify the EPUB's OPF file.

```
unzip epub-sample.epub
vivliostyle build item/standard.opf -s A5 --user-style epub-style.css -o epub-sample.pdf
```

## Generate PDF from Markdown

When a Markdown file is specified with the `vivliostyle build` command, a PDF file will be output as a result of typesetting from the Markdown.

```
vivliostyle build manuscript.md -s A4 -o paper.pdf
```

### About the VFM (Vivliostyle Flavored Markdown)

For more information on the Markdown notation available in Vivliostyle CLI, refer to [VFM: Vivliostyle Flavored Markdown](https://vivliostyle.github.io/vfm/#/).

### Specify a theme CSS style sheet

The `-T` (`--theme`) option can be used to specify a CSS file.

```
vivliostyle build manuscript.md --theme my-theme/style.css -o paper.pdf
```

## Preview the typesetting results

The `vivliostyle preview` command can be used to preview the typesetting results in the browser with [Vivliostyle Viewer](./vivliostyle-viewer.md).

```
vivliostyle preview index.html
```

```
vivliostyle preview https://example.com --user-style my-style.css
```

```
vivliostyle preview publication.json
```

```
vivliostyle preview epub-sample.epub --user-style my-style.css
```

```
vivliostyle preview manuscript.md --theme my-theme/style.css
```

### Quick preview for large multi-document publications

To preview quickly a large publication consisting of multiple HTML documents, specify `-q` (`--quick`) option for quick loading using rough page count (page numbers will not be output correctly).

```
vivliostyle preview index.html --quick
```

```
vivliostyle preview publication.json --quick
```

```
vivliostyle preview epub-sample.epub --quick
```

## About Vivliostyle Themes

- [Vivliostyle Themes](https://vivliostyle.github.io/themes/)

### Find themes

To find the themes published as npm packages, search for the keyword "vivliostyle-theme" in [npm](https://www.npmjs.com/):

- [List of Themes (npm)](https://www.npmjs.com/search?q=keywords%3Avivliostyle-theme)

### Install a theme

To use a theme published as a npm package in Vivliostyle CLI, you need to install it.

```
npm install @vivliostyle/theme-techbook
```

### Using the Create Book

A easier way to use Vivliostyle Themes is to use Create Book. See [Create Book](create-book).

## Configuration file vivliostyle.config.js

To combine multiple article or chapter files into a single publication, use the configuration file. When running the `vivliostyle build` or `vivliostyle preview` command, the configuration file `vivliostyle.config.js` will be used if it exists in the current directory.

### Create configuration file

You can create a configuration file `vivliostyle.config.js` with the following command:

```
vivliostyle init
```

This will create `vivliostyle.config.js` in the current directory. The configuration file is written in JavaScript, and various settings can be changed by editing it.

### Configuration settings

The configuration settings are explained in the comments (beginning with `//`) in the configuration file.

- **title**: Publication title. Example: `title: 'Principia'`.
- **author**: Author name. Example: `author: 'Isaac Newton'`.
- **language**: Language. Example: `language: 'en'`. If this is specified, it will be reflected in the `lang` attribute of the HTML.
- **size**: Page size. Example: `size: 'A4'`.
- **theme**: Specify a CSS file. Example: `theme: 'style.css'`, or specify a [Vivliostyle Themes](https://vivliostyle.github.io/themes/) package name. Example: `theme: '@vivliostyle/theme-techbook'`.
- **entry**: Specify input Markdown or HTML files.
    ```js
    entry: [
      'introduction.md',
      'chapter1.md',
      'chapter2.md',
      'glossary.html'
    ],
    ```
    - You can specify `title` and `theme` for individual items in the `entry`.
        ```js
        entry: [
          {
            path: 'about.md',
            title: 'About This Book',
            theme: 'about.css'
          },
          ...
        ],
        ```
- **output**: Output path(s). Example: `output: 'output.pdf'`. The default is `{title}.pdf`. It is also possible to specify multiple outputs as follows:
    ```js
    output: [
      './output.pdf',
      {
        path: './book',
        format: 'webpub',
      },
    ],
    ```
    For `webpub` output, see [Web Publications (webpub)](#web-publications-webpub).
- **workspaceDir**: Specify the directory where intermediate files are saved. If this is not specified, the default is the current directory, and the HTML files converted from Markdown will be stored in the same location as the Markdown files. Example: `workspaceDir: '.vivliostyle'`.
- **toc**: If `toc: true` is specified, the HTML file `index.html` containing the table of contents will be output. See [Creating a Table of Contents](#creating-a-table-of-contents) for details.

## Generate PDF for print (PDF/X-1a format)

You can use `vivliostyle build` command with the `--press-ready` option to output in PDF/X-1a format suitable for printing.

To use this feature, you need [Docker](https://docs.docker.com/get-docker/) installed.

## Generate PDF Bookmarks

The PDF output from the `vivliostyle build` command will have a table of contents as PDF Bookmarks, which can be used for navigation in PDF viewing software such as Adobe Acrobat.

Bookmark generation is enabled when the publication contains a table of contents. In the case of [Generate PDF from EPUB](#generate-pdf-from-epub), the table of contents in the EPUB will be used. For other cases, see [Creating a Table of Contents](#creating-a-table-of-contents) below.

## Creating a Table of Contents

### Specifying table of contents generation in configuration file

If `toc: true` is specified in the configuration file `vivliostyle.config.js`, a table of contents HTML file `index.html` will be generated and it will be the first file in the publication.

To specify the file name of the table of contents HTML, specify it with `toc:`. Example: `toc: 'toc.html'`

The generated table of contents HTML file will look like the following:

```html
<html>
  <head>
    <title>Book Title</title>
    <link href="publication.json" rel="publication" />
    <link href="style.css" rel="stylesheet" />
  </head>
  <body>
    <h1>Book Title</h1>
    <nav id="toc" role="doc-toc">
      <h2>Table of Contents</h2>
      <ol>
        <li><a href="prologue.html">Prologue</a></li>
        <li><a href="chapter1.html">Chapter 1</a></li>
        <li><a href="chapter2.html">Chapter 2</a></li>
        <li><a href="chapter3.html">Chapter 3</a></li>
        <li><a href="epilogue.html">Epilogue</a></li>
      </ol>
    </nav>
  </body>
</html>
```

### Specifying the title of the table of contents

- The `title` and `h1` elements of the Table of Contents will have the publication title (specified with `title` in the configuration file).
- The title of the table of contents (the content of the heading `h2` element in the `nav` element, "Table of Contents") can be specified with `tocTitle` in the configuration file. Example: `tocTitle: 'Contents'`.

### To output the table of contents to a location other than the top of the publication

If you specify `{ rel: 'contents' }` as an element of the `entry` array in the configuration file `vivliostyle.config.js`, the table of contents HTML file will be generated at that position.

```js
  entry: [
    'titlepage.md',
    { rel: 'contents' },
    'chapter1.md',
    ...
  ],
  toc: 'toc.html',
```

This way, the first HTML file in the publication will be `titlepage.html`, followed by the table of contents HTML file `toc.html`.

### Creating table of contents yourself

To create a table of contents yourself, specify the path to the table of contents file with `rel: 'contents'` in the the `entry` array in the configuration file, as follows:

```js
  entry: [
    'titlepage.md',
    {
      path: 'toc.html',
      rel: 'contents'
    },
    'chapter1.md',
    ...
  ],
```

For information on table of contents structure, please refer to the appendix [Machine-Processable Table of Contents](https://www.w3.org/TR/pub-manifest/#app-toc-structure) of [W3C Publication Manifest](https://www.w3.org/TR/pub-manifest/).

## Web Publications (webpub)

The `vivliostyle build` command with the `-f` (`--format`) option with `webpub` will generate a web publication (webpub). The output destination `-o` (`--output`) option specifies the directory where the webpub will be placed.

(In the example below, we assume that the input Markdown and HTML files are specified in the configuration file `vivliostyle.config.js`.

```
vivliostyle build -o webpub/ -f webpub
```

In the generated webpub directory, there is a publication manifest file, `publication.json`, which contains information about the order in which the content HTML files are loaded. It conforms to the W3C standard specification [Publication Manifest](https://www.w3.org/TR/pub-manifest/).

The webpub can be used to create publications that can be read on the web. You can also generate a PDF from webpub by specifying the `publication.json` file to the `vivliostyle build` command as follows:

```
vivliostyle build webpub/publication.json -o pdfbook.pdf
```

It is also possible to generate both webpub and PDF with a single `vivliostyle build` command, as follows:

```
vivliostyle build -o webpub/ -f webpub -o pdfbook.pdf -f pdf
```

## Other options

You can use the `vivliostyle help` command to display the list of options available in Vivliostyle CLI.

```
vivliostyle help
vivliostyle help init
vivliostyle help build
vivliostyle help preview
```

See also:
- [Vivliostyle CLI (README)](https://github.com/vivliostyle/vivliostyle-cli/blob/main/README.md#readme)
