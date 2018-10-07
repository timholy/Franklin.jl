# JuDoc

| Status | Coverage |
| :----: | :----: |
| [![Build Status](https://travis-ci.org/tlienart/JuDoc.jl.svg?branch=master)](https://travis-ci.org/tlienart/JuDoc.jl) | [![codecov.io](http://codecov.io/github/tlienart/JuDoc.jl/coverage.svg?branch=master)](http://codecov.io/github/tlienart/JuDoc.jl?branch=master) |

## What's this about?

JuDoc is a simple static site generator oriented towards technical blogging and written in Julia.

This:

<!-- =========== EXAMPLE =========== -->
```md
@def title = "Example"

\newcommand{\R}{\mathbb R}
\newcommand{\E}{\mathbb E}
\newcommand{\scal}[1]{\langle #1 \rangle}

# JuDoc Example

You can define commands in a same way as in LaTeX, and use them in the same
way: $\E[\scal{f, g}] \in \R$.
Maths display is done with [KaTeX](https://katex.org).

The syntax is basically an extended form of [gfm](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
allowing for some LaTeX as well as div blocks:

@@box
Something inside a div with div name "box"
@@

You can add figures, tables, links and code just as you would in gfm.

## Why?

Extending Markdown allows to define macros for things that may appear many
times in the current page (or in all your pages), for example let's say you
want to define an environment for systematically inserting images from a
specific folder within a specific div.

\newcommand{\smimg}[1]{@@smimg ![](/assets/smimg/!#1) @@}

\smimg{myimg.png}

It also allows things like referencing (which is not natively supported by
KaTeX for instance):

$$ \exp(i\pi) + 1 = 0 \label{a nice equation} $$

can then be referenced as such: \eqref{a nice equation} which is convenient for
maths notes.
```
<!-- =========== end EXAMPLE =========== -->

Renders to [this page](https://tlienart.github.io/misc/judoc-example1.html).

Like it? read on.

## Getting started

### Installation

To install JuDoc, you need [Julia](https://julialang.org/) (0.7 or above) and JuDoc (which is currently unregistered):

```julia
] # enter package mode
(v 1.0) > add https://github.com/tlienart/JuDoc.jl
```

#### Rendering

To render your site locally, `JuDoc.serve()` (see [judoc.jl](https://github.com/tlienart/JuDoc.jl/blob/master/src/manager/judoc.jl)) uses [`browser-sync`](https://browsersync.io/) which allows you to directly see the modifications you make in your browser.
It is not necessary to install it i.e.: you could run JuDoc, push the pages on GitHub and see how they get rendered there.
However it is easier to see how your website renders locally with any modifications you make being applied live.

To install [`browser-sync`](https://browsersync.io/) just run

```
npm install -g browser-sync
```

(which requires you to have [`npm`](https://www.npmjs.com/get-npm) but it's unlikely you don't.)

#### Minifying

The files generated by JuDoc are pretty simple and thus pretty light already but they can still be compressed a bit more.
For this we use the simple [`css-html-js-minify`](https://github.com/juancarlospaco/css-html-js-minify) which can be installed via `pip`:

```
pip install css-html-js-minify
```

Again, it's not a required dependency, it is however encoded in `JuDoc.publish()` (see [judoc.jl](https://github.com/tlienart/JuDoc.jl/blob/master/src/manager/judoc.jl)) so if you use that, JuDoc will assume that you have it.

**Remark** there are more sophisticated minifiers out there however the script above is simple and has the added advantage that it doesn't clash with KaTeX which [`html-minifier`](https://github.com/kangax/html-minifier) does in my experience.

### Folder structure

The starting point should be a folder with the following structure (the folders/files marked with a star *must* be
there)

```
site
+-- assets
+-- libs (*)
|   +-- katex
|   +-- prism
+-- src (*)
|   +-- _css (*)
|   +-- _html_parts (*)
|   +-- pages (*)
|   |   +-- folder1 ...
|   |   +-- folder2 ...
|	|	+-- ...
|   +-- config.md
|   +-- index.md
```

After running JuDoc, a few extra folders will appear (marked with a dagger)

```
site
+-- assets
+-- css (†)
+-- libs
+-- pub (†)
|   +-- folder1 ...
|   +-- folder2 ...
+-- src
+-- index.html (†)
```

which contain the files corresponding to the actual website.


## Shortcuts

It quickly becomes convenient to define the following two shortcuts and save them in your bash profile (`~/.bash_profile`):

1. a shortcut for running the engine and serving on port 8000 via browser-sync: `alias jd="using JuDoc; JuDoc.serve()"`
