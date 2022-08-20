# Introduction

```{warning}
This book is under construction.
```

Welcome to **coding for economists**, a guide for economists on what programming is, why it's useful, and how to do it.

The book aims to give you the skills you need to code for economics, while also giving you bits and pieces of information about programming more generally that might be useful to you. It's suitable for complete beginners who have never written any code before. Some of the later chapters are suitable for people who have coded before too.

Naturally, at times, we have made choices regarding what to include, what to omit, and what to recommend. We have given recommendations based on what will serve you best in the long-run. Very occasionally, like the Karate Kid painting Mr Miyagi's fence, you will wonder why such and such a thing is useful for coding in an economics context. But, also like the Karate Kid, you will find that you weren't *just* learning to paint a fence, you were becoming a karate (coding) master.

## What is coding?

Computer programming, or coding, is the process of issuing a series of commands that a computer can understand and execute in order to perform a task. Today, almost all research with some quantitative aspect involves coding.

Imagine a computer as being like a particularly mischievous genie; if your instructions aren't completely unambiguous, the computer won't do what you asked for. So we write out our code, our list of instructions for the computer, very explicitly to make sure *we* can check we wrote what we meant, and that the computer will do it in the way we anticipate. Like a genie though, if you can get the instructions just so, you can create magical things.

## Why should economists learn to code?

Given so much of economics is quantitative, coding is an essential skill for many economists. Many of the tools and packages that have been developed will help you to do better work more productively. It's Pareto improving.

As a skill, programming is enormously valuable *beyond economics* too because it's used across a wide range of domains (this is especially true for general purpose programming languages). Code is widely used across industry, academia, and the public sector in everything from computer games to websites, data science to software applications. And learning basic programming concepts in any language is useful for almost any other programming language. So learning to code is good for your human capital.

Coding is like a superpower; it's both fun and powerful! Head to the first section to get started.

## Reading guide

You can read this book in any order depending on your experience; check the chapter titles to find out what would be most valuable for you. But if you want to start getting to grips with using code for economics, the first four sections are designed to help you get as far as possible as quickly as possible.

## Using the book interactively

As well as reading the book, you can also run the code examples for yourself (this is a great way to learn). You can either copy and paste examples into Python scripts and run them yourself, or you can use some of the interactive options we provide.

### Running examples in scripts

For this, you just need to copy and paste examples into a Python script in Visual Studio Code and run it in an *interactive window* (once you have a working Python environment—more on how to set this up in {ref}`code-preliminaries`). If you're finding it difficult to set up your Python environment, you can always use a cloud service version of Visual Studio Code instead: [Github Codespaces](https://github.com/features/codespaces) and [Gitpod](https://www.gitpod.io/) both provide cheap or free access to a ready-made Visual Studio Code development environment in your browser window. Gitpod has a free tier.

### Running the code from the book directly

There are two ways to run code directly from the book:

1. All pages that have code on can be downloaded to your desktop and run as *Jupyter Notebooks* (which you'll find out more about in {ref}`code-where`). Click the download symbol in the top right hand corner of the page and select '.ipynb', which stands for 'ipython notebook'. You will need to install software on your computer to run Jupyter Notebooks. This book recommends the Anaconda distribution of Python, which provides an installation of the Python language for your computer. To write and run code and notebooks, this book recommends [Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial) (plus its Python extension). There's more on how to get started with Anaconda Python and Visual Studio Code together in the next chapter.

2. Most pages that have code on can be run as a *Google Colab* notebook with a single click on `Colab`, found under the rocket button at the top of the page. If you need to install any extra packages in a Google Colab session, the syntax is `!pip install packagename` to install a library called 'packagename'. Colab notebooks are run in a browser window.

To run all of the examples in the book, you may need to install additional packages too, depending on what you already have installed on your computer. The Chapter on {ref}`code-preliminaries` will cover how to install additional packages.

```{admonition} Exercise
Try going to the {ref}`code-basics` chapter now and clicking on the rocketship symbol (🚀), then select "Colab". Try running the first few code cells.
```

For a small number of the examples using data, you may need to download the relevant files from this book's Github [repository](https://github.com/aeturrell/coding-for-economists/tree/main/data), but this will always be explained in the relevant subsections.