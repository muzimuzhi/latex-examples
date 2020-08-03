# LaTeX examples

## [`utilities/code-with-output.tex`](utilities/code-with-output.tex)

### User Interface
 - `\begin{example}[tcb options]{title}`, code followed by output, numbered 
 - `\begin{example*}[tcb options]{title}`, unnumbered variant

Typical configured usage
 - side by side, `\begin{example}[sidebyside]{title}`
 - change code language, `\begin{example}[minted options app={language=python}]{title}`

### Internals
 - direct dependencies: 
   - `tcolorbox`, with libraries `hooks`, `minted`, `skins` and `xparse` loaded
   - `accsupp`
 - environments are based on `tcolorbox`'s  `minted` library, `-shell-escape` required
 - added
   - `\emptyaccsupp`
   - `tcolorbox` options `example options` and `example title`
 - modified
   - `\theFancyVerbLine`

## [`utilities/print-definition.tex`](print-definition.tex)

### User Interface
 - `\printDef{csname}`, print definition of `\cs{csname}`
 - `\printAndRunCode{code}`

### Internals
 - direct dependencies:
   - `fvextra`
   - `xcolor` with no package options
 - added
   - `\toString`
