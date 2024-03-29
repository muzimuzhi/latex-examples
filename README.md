# LaTeX examples

- Dec 13, 2022
    Rewrote whole Git history to track all PDF files with Git LFS.\
    (by executing `git lfs migrate import --include="*.pdf" --everything`).

## Utilities

### [`code-with-output.tex`](utilities/code-with-output.tex)

User Interface
 - `\begin{example}[tcb options]{title}`, code followed by output, numbered 
 - `\begin{example*}[tcb options]{title}`, unnumbered variant

Typical configured usage
 - side by side, `\begin{example}[sidebyside]{title}`
 - change code language, `\begin{example}[minted options app={language=python}]{title}`

Internals
 - direct dependencies: 
   - `tcolorbox`, with libraries `hooks`, `minted`, `skins` and `xparse` loaded
   - `accsupp`
 - environments are based on `tcolorbox`'s  `minted` library, `-shell-escape` required
 - added
   - `\emptyaccsupp`
   - `tcolorbox` options `example options` and `example title`
 - modified
   - `\theFancyVerbLine`

### [`pgfkeys` library `conditionals`](utilities/pgfkeyslibraryconditionals.code.tex)

User Interface
  - loading: `\usepgfkeyslibrary{conditionals}`
  - conditional keys
    - `/utils/ifname nTF={<name>}{<true options>}{<false options>}`
      executes `\if<name> ... \else ... \fi`
    - `/utils/ifxxx nTF={<ifxxx header>}{<true options>}{<false options>}`
      executes `<ifxxx header> ... \else ... \fi`
      - `/utils/ifnum nNnTF={<int expr a>}<relation>{<int expr b>}{<t>}{<f>}`
        (trick: hide `<relation>` `=` in braces, like `/utils/ifnum 1{=}1tf`)
      - `/utils/ifdim nNnTF={<dim expr a>}<relation>{<dim expr b>}{<t>}{<f>}`
      - `/utils/if NNTF=<token a><token b>{<t>}{<f>}`
        (`\noexpand` auto applied to `<tokan a>` and `<token b>`)
      - `/utils/ifcat NNTF=<token a><token b>{<t>}{<f>}` (ditto)
      - `/utils/ifx NNTF=<token a><token b>{<t>}{<f>}`
      - `/utils/if strcmp nTnTF={<text a>}<relation>{<text b>}{<t>}{<f>}`
        (`<text a/b>` are auto wrapped by `\unexpanded`; relies on primitive `\(pdf)strcmp` or its mock in Lua)

Internals
  - prefix `\pgfkeys@lib@cond@xxx`

TODO
  - export to other default paths\
    to support uses like `\tcbset{ifnum TF=...}` with no need to set `\pgfkeys{/tcb/.see also={/utils}}`.  Also more space consumption for shorter time.
    - utility to copy a key (need to learn if `filtered` library adds any internal sub-keys)

### [`hyperref-autonameref.tex`](utilities/hyperref-autonameref.tex)

User Interface
  - `\autonameref{<label key>}` and `\autonameref*{<label key>}`
  - 1-arg `\HyRef@autonameref@style` which controls the extra output style (see [test file](test/hyperref-autonameref-test.tex))

Internals
  - direct dependency: `hyperref`
  - `\HyRef@autonameref` and `\HyRef@autonamesetref`


### [`pgfkeys-handler-patch.tex`](utilities/pgfkeys-handler-patch.tex)

User Interface
 - `\pgfkeys{<key>/.patch={<search>}{<replace>}}`
 - `\pgfkeyspatchvalue{<key path>}{<search>}{<replace>}`

Internals
 - direct dependency: `xpatch`


### [`pgfkeys-handler-store-in.tex`](utilities/pgfkeys-handler-store-in.tex)

User Interface
 - after `<key>/.store in=<macro>` (or `.estore in`), handlers `.get`, `.add`, `.prefix`, and `.append` will act on `<macro>`, not the key itself

Internals
 - `<macro>` is stored in new sub-key `.@store`, which will be cleared by `.initial`
 - for the above four handlers, `.@store` has higher precedence than the key itself (set by `.initial`)

### [`print-definition.tex`](utilities/print-definition.tex)

User Interface
 - `\printDef{csname}`, print definition of `\cs{csname}`
 - `\printAndRunCode{code}`

Internals
 - direct dependencies:
   - `fvextra`
   - `xcolor` with no package options
 - added
   - `\toString`


### [`tikz-auto-mark-nodes.tex`](utilities/tikz-auto-mark-nodes.tex)

User Interface
 - scope options `auto mark` and `no auto mark`
 - styles `every auto mark` and `every auto <shape> mark` that accept `pin` options
 - zero-arg macro `\tikzAutoMarkText` that controls the mark text
   - In the definition of the above styles and macro, `\tikzNodeName` and `\tikzNodeShape` can be used as placeholders of node name and shape, respectively.

Initial values
  - .
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

Internals
 - Every auto mark is a node pin drawn by 
    ```tex
    \node also[pin={[every auto mark/.try, every auto <shape> mark/.try]{\tikzAutoMarkText}}] (\tikzNodeName);
    ```
    at the end of every `tikzpicture`.
    - maybe draw in `execute at end path`?
 - direct dependency: `tikz` and `etoolbox` (for `\patchcmd`)
 - `tikz` options used: `execute at begin scope` and `execute at end picture`
 - patched: `\tikz@node@finish` to append node info to `\tikzNodeList`
 - added:
    - `\tikzNodeList`, A comma-separated list of elements `{<node_name>, <node_shape>}`
    - `\newif\iftikz@lib@automark@on`

### [`unique-csname.tex`](utilities/unique-csname.tex)

User Interface
 - `\undefine<cs>` and `\undefinecs{<csname>}` un-define a control sequence locally
 - `\uniquecsname` expands to a `<csname>` which tests false in `\ifcsname`

Internals
 - `\@uniquecsname@try` and `\@uniquecsname@generate`

### [`xcolor-quick-variants.tex`](utilities/xcolor-quick-variants.tex) (wip)

User Interface
 - `\defineqcolor{<name>}{<model>}{<spec>}`
 - `\colorqlet{<name1>}{<name2>}`
 - `\qcolor{<name>}` or `\qcolor[<model>]{<spec>}`
   - `<model> ::= gray | rgb | cmyk`

Internal
 - `\XC@nametocs{<color name>}`
