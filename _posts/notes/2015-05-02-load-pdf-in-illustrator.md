---
layout: article
title: "Loading pdf file as a vector graphics in Adobe Illustrator"
date: 2015-05-02T17:11:28-0700
modified:
categories: notes
comments: true
excerpt:
tags: [illustrator]
ads: true
image:
  feature:
  teaser:
---

Importing LaTeX compiled pdf files sometimes fail because of missing LaTeX
fonts. In that case, follow the procedure below to convert the pdf files into
vector graphics (including all texts) to import fonts as vector graphics[^1].

- Click “File”, then click “Place” to load a PDF page.
- Select the loaded page and click “Object” and select “Flatten Transparency”.
- On the pop up menu, check “Convert All Text as Outlines”

You can see all the fonts are imported as paths. To save,

- Click “File”, then click “Save as”
- Select “svg” format and click “Save”
- On the pop up menu, select “More options”
- Select decimal places to be at least 2
- Click “Ok”

[^1]: http://www.shabishine.com/2013/03/how-to-open-pdf-file-in-illustrator.html
