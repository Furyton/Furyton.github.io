---
title: Notes about Technical Writing
author: Furyton
date: 2022-08-09
categories: [writing, note]
---

# basic latex

-   extra spaces between words and extra blank lines between passages
    will be ignored.

    -   spaces in the front will be ignored.

    -   newline is regarded as an extra space, adding `%` at the end
        will remove it.

    -   note: space before macros will not be ignored, e.g.,
        ` \TeX ing`. But spaces at the rear will be ignored. Adding
        brankets `{}` will resolve it, e.g., ` {\TeX} ing` or
        ` \Tex{} ing`.

-   `\qquad`: as wide as about two `M` s

-   double hyphen: `--` denotes number range, en dash; triple hyphen:
    `---` denotes punctuation dash, em dash. en and em denotes the width

-   float environment: e.g., `figure`, `table`. accept an optional arg
    (`h`: here; `t`: top; `H`: here and not float, extended from package
    `float`)

-   `\eqref` is specified for math equations from package `amsmath`

-   avoid setting font or controlling indents, etc., within the document
    environment. try to replace them with meaningful commands or
    environments.

    -   `\newenvironment{myenv}`

    -   `\newcommand`

-   ellipsis: `\ldots` or `\dots`. instead of `\cdots` or `...`(three
    dots).

    -   `H\dots.` (OK)

    -   `H \ldots H` (not good), `H $\ldots$ H` (recommended, math env)

-   escape chars in the main body.

    -   `\_` for `_`; `\textbackslash` for `\`

-   non-breakable space.

    -   `Question~1`

    -   `Donald~E. Knuth` within names

    -   `Mr.~Knuth`

    -   `function~$f(x)$`

    -   `1,~2, and~3`

-   period after capital is regarded as a abbreviation. use `\ ` or `\@`
    to resolve it. E.g., `Roman number XII\@. Yes.`

-   BibTeX

    -   `\citep` (index) and `\citet` (author) are recommended. remember
        to use package `natbib` and use `plainnat` bibliography style

-   force line break

    -   `\\` accepts an optional argument for vertical space. e.g.,
        `\\[2cm]`. Often used for in equation environment

-   special chars used in main body

    -   §: `\S`

    -   : `\dag`

    -   : `\ddag`

    -   ¶: `\P`

    -   ©: `\copyright`

    -   £: `\pounds`

    -   $\bullet$: `\textbullet` (`\bullet` in math env)

    -   go check the book [**The Comprehensive LATEX Symbol List,
        2009**](/www/The-Comprehensive-LATEX-Symbol-List.pdf) by
        Scott Pakin for more

# fonts

## font family

-   roman family: `\textrm{font family}` [font family]{.roman}

-   sans-serif family: `\textsf{font family}` [font family]{.sans-serif}

-   typewritter family: `\texttt{font family}` `font family`

declaration, `{\rmfamily font family}` font family

## font shape

-   upright shape: `\textup{Font Shape}` [Font Shape]{.upright}

-   italic shape: `\textit{Font Shape}` *Font Shape*

-   slanted shape: `\textsl{Font Shape}` *Font Shape*

-   small captical shape: `\textsc{Font Shape}` [Font Shape]{.smallcaps}

declaration: `{\itshape Font Shape}` *Font Shape*

## font series

-   medium series: `\textmd{font series}` [font series]{.medium}
    (default in main body)

-   bold series: `\textbf{font series}` **font series**

declaration: `{\mdseries font series}` font series

## font typeface

-   The default font family of $\LaTeX$ is *Computer Modern*

-   *Serif Times Roman* (i.e., *Times New Roman*) is recommended for
    papers, magazines and books. Use package `txfonts`

-   *Concrete* is recommended for presentation. package combination
    `ccfonts`, `eulervm` is great. (also `arec` ,`cmbright`)

we can specify three font family individually

    \usepackage{fontspec}
    \setmainfont{Times New Roman}
    \setsansfont{Verdana}
    \setmonofont{Courier New}

## emphasis

make upright or make *italic* upright

-   `this is \emph{emphasis}`: this is *emphasis*

-   `\textit{this is \emph{emphasis}}` *this is *emphasis**

underline

`\underline{Emphasized} text and \underline{another}`:
[Emphasized]{.underline} text and [another]{.underline}

use package `\usepackage[normalem]{ulem}`

`\uline{Emphasized} text and \uline{another}`: [Emphasized]{.underline}
text and [another]{.underline}

## font size

