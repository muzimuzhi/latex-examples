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
