---
title: How to Edit this Website
sidebar: mydoc_sidebar
permalink: how_to_edit.html
folder: mydoc
---

<br>

This website was built using the [Documentation Theme for Jekyll](https://github.com/matuzalemmuller/documentation-theme-jekyll) and pages can be edited using the [Markdown](https://en.wikipedia.org/wiki/Markdown) and [HTML](https://wikipedia.org/wiki/HTML) languages.

## Editing existing page

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

## Adding page to sidebar navigation

After the page is created, it can be added to the sidebar. To add a shorcut in the sidebar, edit the file `_data/sidebars/mydoc_sidebar.yml`:

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

<center>{% include image.html file="Example_Page.png" alt="Jekyll" caption="Example page" %}</center>