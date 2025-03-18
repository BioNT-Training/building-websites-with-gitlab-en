---
title: GitLab Pages with Jupyter books
teaching: 0
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Publish Jupyter notebooks as HTML on the web with GitHub Pages

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I publish web pages through GitLab and Jupyter books?

::::::::::::::::::::::::::::::::::::::::::::::::::

Let's do something different, that will sound a little more familiar if you are used to the
[Jupyter](https://jupyter.org/) projects. From their website:

> Project Jupyter exists to develop open-source software, open-standards, and services for interactive computing across
> dozens of programming languages.

**Jupyter notebooks** allow you to create and share documents containing live code as well as narrative text.
**Jupyter Lab** expand their functionalities by creating and interactive development environment (allowing you to
navigate your file system and define the coding environment). **Jupyter Hub** is a multi-user version of Jupyer Lab
that institutions can implement locally ([as we at EMBL did](https://jupyterhub.embl.de/)).

And **Jupyer book**?

> Jupyter Book is an open source project for building beautiful, publication-quality books and documents from
> computational material.

The [extensive tutorial](https://jupyterbook.org/start/your-first-book.html) will guide you through the installation
steps and detailed customisation available,  in the context of this lesson we are going to cover just the basics.
First things first, let's install Jupyter Book. In your terminal:

```bash 
pip install -U jupyter-book
```

Please note: you will need to have **pip** installed too. Now, let's create our first book project. The
`jupyter-book --help` command would help us in checking the options here, but this lesson will spoiler something: the

```bash 
jupyter-book create jupyter-book-demo
```

command will create a basic Jupyter book in a `jupyter-book-demo` folder. This folder already includes the three things
needed for building a book: a `_config.yml` file, a `_toc.yml` table of content and the book's content in a collection
of MarkDown, reStructuredText or Jupyter Notebook files.

Since we are getting tired of all of this development and deployment, we don't really want to edit anything in the
content, but we prioritise having it up and running in GitLab instead. Just guess, what do we need to add? A
`.gitlab-ci.yml` file indeed.

```yaml 
pages:
  stage: deploy
  image: python:slim
  script:
    - pip install -U jupyter-book
    - jupyter-book clean .
    - jupyter-book build .
    - mv _build/html public
  artifacts:
    paths:
      - public
```

This piece of code:

1. Installs jupyter-book (or checks that it is correctly installed remotely).
2. Cleans the folder from files resulting from (eventual) previous builds.
3. runs the command `jupyter-book build .`, which builds the book in the folder into a `_build`
  subfolder. You can check the output by running the same command in your terminal, and you will realise that the
  actual HTML files are in the subfolder `_build/html`.
4. It then moves the HTML content into our usual `public` folder where the artifacts are stored.

:::::::::::::::::::::::::::::::::::::::  challenge

## Your time to experiment with a template

This template is deliberately minimal to give you the opportunity to test your documentation reading skills.
Check the topic guides at [jupyterbook.org](https://jupyterbook.org/intro.html) and find a way to:

1. Add another page called "About" and linked from the table of contents.
2. Play with the file format of this new page, add the same type of content in MarkDown, reStructuredTex and Notebook formats.
3. Add one figure and a figure caption.
4. Insert a code cell. If you are familiar with any programming language, add a simple plot and visualise the output of such plot into your page.
5. For more advanced code features, check [how to make the code executable](https://jupyterbook.org/interactive/thebe.html)
6. Check the [gallery of Jupyter books](https://executablebooks.org/en/latest/gallery.html) for inspiration!
  

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Through Jupyter books, you'll be able to integrate interactive components and code in your web pages

::::::::::::::::::::::::::::::::::::::::::::::::::


