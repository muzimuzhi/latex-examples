# Syntax of `\Figure` and `\Table`, new commands in ieeeaccess latex template

## Usages of latex2e float environments

```tex
\begin{figure(*)}[<position>]
  \centering
  \includegraphics[<graphicx_keys>]{<image_file>}
  \caption{<caption>}\label{<label>}
\end{figure}

\begin{table(*)}[<position>]
  \centering
  \caption{<caption>\label{<label>}}
  <tab_contents>
\end{table}
```


## Full grammar of `\Figure` and `\Table`

```tex
\Figure
  [<position>]                % optional
  (<fig_keys>)                % optional
  [<graphicx_keys>]           % optional
  {<image_file>}              % mandatory
  {<caption>\label{<label>}}  % mandatory

\Table
  [<position>]                % optional
  (<tab_keys>)                % optional
  {<caption>\label{<label>}}  % mandatory
  {<tab_contents>}            % mandatory
```

Examples

```tex
\Figure[!h][width=3cm]{example-image}{caption text\label{fig:a}}

\Table[H]{caption text\label{tab:a}}{%
  \begin{tabular}{ll} a & b \\ c & d \end{tabular}%
}
```

## Grammar of new optional arguments

```abnf
fig_keys  ::= fig_key ("," fig_key)*
fig_key   ::= ("topskip" | "medskip" | "botskip" ) "=" dim
tab_keys  ::= tab_key ("," tab_key)*
tab_key   ::= ("topskip" | "medskip" | "width" | "resize") "=" dim 
            | "arraystretch" "=" num
```


## Default values of optional arguments

```abnf
; Default values of optional arguments, document level
fig_keys      ::= "topskip=0pt, midskip=0pt, botskip=0pt"
tab_keys      ::= "topskip=0pt, botskip=0pt, width=\columnwidth, resize=!, arraystretch=1.3"

; Default values of optional arguments, command level
position      ::= "t!"
fig_keys      ::= "topskip=0pt"
tab_keys      ::= "topskip=0pt"
graphicx_keys ::= "scale=1"
```


## Pseudocode of `\Figure` and `\Table`

```tex
let <fig_box> = \includegraphics[<graphicx_keys>]{<image_file>}
if (width of <fig_box> less than \columnwidth)
  let <fig_env> = "figure"
else
  let <fig_env> = "figure*"
fi
typeset
  \begin{<fig_env>}[<position>]
    \centering
    \vspace*{5pt + <topskip>}
    <fig_box>
    \vspace*{<midskip>}
    \caption{<caption>\label{<label>}}
    \vspace*{<botskip>}
  \end{<fig_env>}
end typeset
```

```tex
if (<width> less than \columnwidth)
  let <tab_env> = "table"
else
  let <tab_env> = "table*"
fi
typeset
  \begin{<tab_env>}[<position>]
    \centering
    \renewcommand\arraystretch{<arraystretch>}
    \parbox{<width>}{\caption{<caption>\label{<label>}}
    if (<resize> is "!")
      <tab_contents>
    else
      \resizebox{<resize>}{!}{<tab_contents>}
    fi
  \end{<tab_env>}
end typeset
```

## References
 - Source file `ieeeaccess.cls`, see  https://journals.ieeeauthorcenter.ieee.org/create-your-ieee-article/authoring-tools-and-templates/ieee-article-templates/templates-for-ieee-access/
 - Syntax notation used by Python language reference, see https://docs.python.org/3/reference/introduction.html#notation
