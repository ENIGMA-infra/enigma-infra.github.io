# enigma-infra.github.io
An overview website of projects, progress, and documentation 

To edit an existing page,
go into the `docs/` folder and edit the corresponding markdown file.
To add a new page, 
add a new entry in the `nav` section of the `mkdocs.yml` file,
and then create a new markdown file in the `docs/` folder.

To edit or make a blog post,
add a new markdown file in the `docs/blog/posts` folder.
A blog post must have a markdown header with a date like so:

```markdown
---
date: 2025-08-29
---
```
It can also have other types of metadata. 
See the [mkdocs material docs for more information](https://squidfunk.github.io/mkdocs-material/tutorials/blogs/basic/?h=blog#basic-blogs).


To locally build the site, 
you need a python local environment:

The first time, do this:
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Then to build the website locally, run
```bash
mkdocs serve
```
and open your browser at the URL shown in the terminal.
Typically, it is `http://localhost:8000`.


