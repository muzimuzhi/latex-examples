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


## [`utilities/print-definition.tex`](utilities/print-definition.tex)

### User Interface
 - `\printDef{csname}`, print definition of `\cs{csname}`
 - `\printAndRunCode{code}`

### Internals
 - direct dependencies:
   - `fvextra`
   - `xcolor` with no package options
 - added
   - `\toString`


## [`utilities/pgfkeys-handler-patch.tex`](utilities/pgfkeys-handler-patch.tex)

### User Interface
 - `\pgfkeys{<key>/.patch={<search>}{<replace>}}`
 - `\pgfkeyspatchvalue{<key path>}{<search>}{<replace>}`

### Internals
 - direct dependency: `xpatch`


## [`utilities/pgfkeys-handler-store-in.tex`](utilities/pgfkeys-handler-store-in.tex)

### User Interface
 - after `<key>/.store in=<macro>` (or `.estore in`), handlers `.get`, `.add`, `.prefix`, and `.append` will act on `<macro>`, not the key itself

### Internals
 - `<macro>` is stored in new subkey `.@store`, which will be cleared by `.initial`
 - for the above four handlers, `.@store` has higher precedence than the key itself (set by `.initial`)


## [`utilities/hyperref-autonameref.tex`](utilities/hyperref-autonameref.tex)

### User Interface
  - `\autonameref{<label key>}` and `\autonameref*{<label key>}`
  - 1-arg `\HyRef@autonameref@style` which controls the extra output style (see [test file](test/hyperref-autonameref-test.tex))

### Internals
  - direct dependency: `hyperref`
  - `\HyRef@autonameref` and `\HyRef@autonamesetref`


## [`utilities/tikz-auto-mark-nodes.tex`](utilities/tikz-auto-mark-nodes.tex)

### User Interface
 - styles `every auto mark` and `every auto <shape> mark` that accept `pin` options
 - zero-arg macro `\tikzAutoMarkText` to control the mark text
 - In the definition of the above styles and macro, `\tikzNodeName` and `\tikzNodeShape` can be used as placeholders of node name and shape, respectively.

### Initial values
    ```tex
    \tikzset{
      every auto mark/.style={
        font=\ttfamily, rotate=45,
        red, anchor=west, pin position=45,
      },
      every auto coordinate mark/.style={
        blue, anchor=east, pin position=180+45,
      },
    }
    \newcommand\tikzAutoMarkText{\tikzNodeName}
    ```

### Internals
 - Every auto mark is a node pin drawn by 
    ```tex
    \node also[pin={[every auto mark/.try, every auto <shape> mark/.try]{\tikzAutoMarkText}}] (\tikzNodeName);
    ```
    at the end of every `tikzpicture`.
    - maybe `scope` is better?
    - fails with nesting 
 - direct dependency: `tikz` and `etoolbox` (for `\xappto`)
 - `tikz` option used: `every picture`, which uses `evecute at begin/end picture`
 - patched: `\tikz@node@finish` to append node info to `\tikzNodeList`
 - added:
    - `\tikzNodeList`, A comma-separated list of elements `{<node_name>, <node_shape>}`
    - `\newif\if@TikzNodeRecord@`