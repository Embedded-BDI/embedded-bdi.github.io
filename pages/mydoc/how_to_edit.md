---
title: How to Edit this Website
sidebar: mydoc_sidebar
permalink: how_to_edit.html
folder: mydoc
---

<br>

This website was built using the [Documentation Theme for Jekyll](https://github.com/matuzalemmuller/documentation-theme-jekyll), and pages can be edited using the [Markdown](https://en.wikipedia.org/wiki/Markdown) and [HTML](https://wikipedia.org/wiki/HTML) languages.

## Editing an existing page

Files for existing pages can be found and edited in the `pages/mydoc` folder.

## Creating a new page

To create a new page, create a new file under the `pages/mydoc` folder. Example:

* File name: `example_page.md`
* Content:
  ```
  ---
  title: Example Page
  sidebar: mydoc_sidebar
  permalink: example_page.html
  folder: mydoc
  ---

  Some text.
  ```

The page will be available at `https://<URL>/example_page.html`

## Adding page to the sidebar navigation

After the page is created, it can be added to the sidebar. To add a shortcut in the sidebar, edit the file `_data/sidebars/mydoc_sidebar.yml`:

* Content:
  ```
  entries:

  - title: sidebar
    product: Embedded-BDI
    version: 1.0
    folders:

    - title: Overview
      output: web, pdf
      folderitems:

      - title: Example page
        url: /example_page.html
        output: web, pdf
  ```

Result:

<center>{% include image.html file="example_page.png" caption="Example page" %}</center>

## Adding attachments

Images are saved in the `images` folder, and other files can be saved on `pages/files`. Images can be added using Markdown or HTML. To add a download link to a file, use the following command.

```
<a href="./pages/files/filename.zip" target="_blank">Link text</a>
```