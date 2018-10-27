**The `tikz-vowel` package**

Christopher Weedall\
The Australian National University\
`chris.weedall@anu.edu.au`

\bigskip
\today
Introduction
============

Two packages exist for drawing vowel charts in LaTeX: Rei Fukui's
`vowel` package (distributed as part of the TIPA package) and Alan
Munn's `pst-vowel` which incorporates drawing abilities from `pstricks`.
The `vowel` package fundamentally lacks the ability to draw diphthongs
on the chart and the triangle diagram seems to be broken for back
vowels. The `pstricks` package has a limitation (that seems to be a bug)
where, when using , the labels disappear when using `\mput`, `\lput`,
`\ncput`, etc. Given the ease in directly inputting the International
Phonetic Alphabet (IPA) via `fontenc` and `fontspec` in , I created this
package as an alternative to `vowel` and `pst-vowel`.

This `tikz-vowel` package aims to overcome some of the `pstricks`
limitations, hide the drawing syntax/mechanics of the `tikz` package,
and add some additional options and commands to make custom vowel and
diphthong locations easier. The package also was created using primarily
LaTeX3, therefore it should work with any documents or packages using
LaTeX3 (a small patch was used to allow it to work with `tikz`). The
package was written entirely from scratch and does not rely upon the
inclusion of either `vowel` or `pst-vowel`.

This documentation describes the `tikz-vowel` environment and the
various vowel and diphthong commands which can be used within that
environment. Users familiar with the `vowel` or `pst-vowel` packages
should have little problem adapting.

\tableofcontents
Requirements {#sec:Requirements}
============

Since the package is based on LaTeX3, it requires `expl3`. The package
also depends on `l3regex` and `xstring` for regular expressions and some
string operations, as well as `xparse` for LaTeX3-friendly new document
commands and the `tikz-vowel` environment.

Generally speaking, this package should compile for most flavors of
LaTeX, such as , , and . The `pst-vowel` package indicates that it must
be compiled with latex+dvips (and not with pdflatex) due to `pstricks`,
so hopefully this package also removes an limitations in that regard.

Lastly, this `tikz-vowel`, as the name suggests, depends on `tikz`. From
the `tikz` package, it also requires `\usetikzlibrary{calc}` and
`\usetikzlibrary{arrows.meta}` for calculations (for the diphthong
arrows and vowel nodes) and the arrows on those (diphthong) lines,
respectively.

Restrictions {#sec:Restrictions}
============

Within the `tikz-vowel` environment, any valid Ti*k*Z code is allowed.
However, if you do directly use Ti*k*Z code, be aware that manual
inclusion of Ti*k*Z commands is not officially supported by `tikz-vowel`
- so your mileage may vary. (Note: the vowel and diphthong commands
provided in the `tikz-vowel` are essentially wrappers for Ti*k*Z code).

Aside from the vowel and diphthong commands provided in the `tikz-vowel`
package, TeXand LaTeXcommands are not supported within the `tikz-vowel`
environment - with the exception of the vowel or diphthong label.
Anything that you include will not be displayed anyway. I recommend you
do not include them, because they could create compilation errors that
you are not expecting.

Including TeXand LaTeXcommands for vowel and diphthong labels (e.g.
`\textbf` and `\textit`) should work, however, this method is not
robustly supported. If you have problems due to using one or more
commands giving errors, best to take them out. If you think it *should*
work, but it does not, contact the `tikz-vowel` maintainer and see if an
update can account for the required command.

Generally speaking, **only** the options and commands included within
`tikz-vowel` should be used. If you are using any other commands, do not
be surprised if unexpected problems arise which are not supported by
`tikz-vowel`.

Ti*k*Z-vowel environment {#sec:TikZ-vowel environment}
========================

Font effects for the `tikz-vowel` environment {#sec:Font effects for the tikz-vowel environment}
---------------------------------------------

One of the benefits to using Ti*k*Z is that the `tikz-vowel` environment
can be modified by font styles and sizes to 'globally' affect all text
appearing within the vowel chart. For example, if you to bold all the
vowels, you can surround `tikz-vowel` environment with `{\bfseries` and
`}`. Or for italics, surround `` environment with `{\itshape` and `}`,
and so forth. Any text commands which allow `\par` can surround the
`tikz-vowel` environment. Note: this means commands such as `\textbf` or
`\textit` *cannot* surround the `tikz-vowel` environment.

In addition to change font appearance, you can change the font size
'globally' for an entire vowel chart. To do so, you would surround the
`tikz-vowel` environment with curly brackets and have the desired font
size command following the opening curly bracket. Example: `{\Large`
`tikz-vowel` environment `}`. Font size changes will affect only the
size of the vowel labels and *not* the size of the entire vowel chart
(see §[4.2](#sec:tikz-vowel environment){reference-type="ref"
reference="sec:tikz-vowel environment"} for more on vowel chart size).

`tikz-vowel` environment {#sec:tikz-vowel environment}
------------------------

\medskip
  ---------------------- ------------------------------------------------------------------
  `\begin{tikz-vowel}`   `[`*option~1~ (, option~2~, ...)*`]`
                         `(`*tikz-option~1~=value~1~ (, tikz-option~2~=value~2~, ...)*`)`
                          
                         
                          
  `\end{tikz-vowel}`     
  ---------------------- ------------------------------------------------------------------

\medskip
Options and commands for inputting vowels (see
§[5](#sec:Vowels){reference-type="ref" reference="sec:Vowels"}) and
diphthongs (see §[6](#sec:Diphthongs){reference-type="ref"
reference="sec:Diphthongs"}) are explained below.

Shapes of the `tikz-vowel` diagram {#sec:Shapes of the tikz-vowel diagram}
----------------------------------

There are three supported shapes for the vowel chart: trapezoid,
`rectangle`, or `triangle`. The trapezoid is the `ipanew` (or
unspecified), used unless `rectangle` or `triangle` is explicitly
specified, and the typical chart shape use for IPA. The trapezoid is
used even if no `tikz-vowel` *option*s are specified. Chart shapes are
mutually exclusive; you cannot, for example specify the chart as both
rectangle and triangle.

\bigskip
The following options create each shape:

  ------------------------------------------------------------------------------------------------------- ------------------------------------------------------------ -----------------------------------------------------------
   `\begin{tikz-vowel}` ... `\end{tikz-vowel}` *or* `\begin{tikz-vowel}[ipanew]` ... `\end{tikz-vowel}`    `\begin{tikz-vowel}` `[rectangle]` ... `\end{tikz-vowel}`    `\begin{tikz-vowel}` `[triangle]` ... `\end{tikz-vowel}`
  (scale=0.75)                                                                                            (scale=0.75)                                                 (scale=0.75)
  ------------------------------------------------------------------------------------------------------- ------------------------------------------------------------ -----------------------------------------------------------

Internal lines/shapes of the `tikz-vowel` diagram {#sec:Internal lines/shapes of the tikz-vowel diagram}
-------------------------------------------------

The following option are allowed in each shape:

-   `plain`\
    No lines drawn within diagram boundaries.

-   `simple`\
    Draws two horizontal lines (i.e. four vowel heights).

-   `standard`\
    Draws three vertical lines from 'turned a' to the top boundary.

-   `three`\
    Draws one horizontal line (i.e. three vowel heights).

Using `three`, `simple`, `standard` supercede the `plain` option. Using
the `three` option automatically supercedes the options `simple` and
`standard`.

The following diagrams and their code illustrate the different internal
lines possible with the trapezoid shape:

  ------------------------------ --
                                 
  `\end{tikz-vowel}`             
  *or*                           
  `\begin{tikz-vowel}[ipanew]`   
  `\end{tikz-vowel}`             
                                 
  `\end{tikz-vowel}`             
                                 
                                 
                                 
                                 
  ------------------------------ --

  -------------------- --
                       
  `\end{tikz-vowel}`   
                       
  `\end{tikz-vowel}`   
                       
                       
                       
                       
  -------------------- --

  -------------------- ---
                       
  `\end{tikz-vowel}`   
                        
                       
                       
  -------------------- ---

The following diagrams and their code illustrate the different internal
lines possible with the `rectangle` shape:

  -------------------- --
                       
  `\end{tikz-vowel}`   
                       
  `\end{tikz-vowel}`   
                       
                       
                       
                       
  -------------------- --

  -------------------- --
                       
  `\end{tikz-vowel}`   
                       
  `\end{tikz-vowel}`   
                       
                       
                       
                       
  -------------------- --

  -------------------- ---
                       
  `\end{tikz-vowel}`   
                        
                       
                       
  -------------------- ---

The following diagrams and their code illustrate the different internal
lines possible with the `triangle` shape:

  -------------------- --
                       
  `\end{tikz-vowel}`   
                       
  `\end{tikz-vowel}`   
                       
                       
                       
                       
  -------------------- --

  -------------------- --
                       
  `\end{tikz-vowel}`   
                       
  `\end{tikz-vowel}`   
                       
                       
                       
                       
  -------------------- --

  -------------------- ---
                       
  `\end{tikz-vowel}`   
                        
                       
                       
  -------------------- ---

Ti*k*Z options for the `tikz-vowel` diagram {#sec:TikZ options for the tikz-vowel diagram}
-------------------------------------------

Literally any option that can be given to `\begin{tikzpicture}` is valid
for the `tikz-vowel` package. You will need to consult the `tikz`
package for details on these options. Keep in mind that these options
apply to the entire vowel chart. The most common that will probably be
used, however, is the *scale* option which allows you to resize the
entire `tikzpicture` (which is equivalent to the entire vowel chart
defined by the `\begin{tikz-vowel}` environment). Whereas surrounding
the `tikz-vowel` environment with a font size will affect the size of
the vowel and diphthong labels, the *scale* option will affect the size
of the chart, the vowel/diphthong labels, the vowel dots, and the length
of the diphthong arrows.

\bigskip
Examples of different size charts, using the *scale* option:

+-----------:+:---------------------------------------------+
| \centering | \vspace{-65pt}                               |
| (scale=1)  |   {\small                                    |
|            | \begin{itemize}[label={}]                    |
|            |     \item \verb|\begin{tikz-vowel}(scale=1)| |
|            |     \item \verb|\end{tikz-vowel}|            |
|            | \end{itemize}                                |
|            |     }                                        |
+------------+----------------------------------------------+

+------------:+:-----------------------------------------------+
| \centering  | \vspace{-45pt}                                 |
| (scale=0.5) |   {\small                                      |
|             | \begin{itemize}[label={}]                      |
|             |     \item \verb|\begin{tikz-vowel}(scale=0.5)| |
|             |     \item \verb|\end{tikz-vowel}|              |
|             | \end{itemize}                                  |
|             |     }                                          |
+-------------+------------------------------------------------+

+-------------:+:------------------------------------------------+
| \centering   | \vspace{-100pt}                                 |
| (scale=1.75) |   {\small                                       |
|              | \begin{itemize}[label={}]                       |
|              |     \item \verb|\begin{tikz-vowel}(scale=1.75)| |
|              |     \item \verb|\end{tikz-vowel}|               |
|              | \end{itemize}                                   |
|              |     }                                           |
+--------------+-------------------------------------------------+

\bigskip
Examples of different line widths, using the *line width* option:

+-----------------:+:----------------------------------------------------+
| \centering       | \vspace{-65pt}                                      |
| (line width=2pt) |   {\small                                           |
|                  | \begin{itemize}[label={}]                           |
|                  |     \item \verb|\begin{tikz-vowel}(line width=2pt)| |
|                  |     \item \verb|\end{tikz-vowel}|                   |
|                  | \end{itemize}                                       |
|                  |     }                                               |
+------------------+-----------------------------------------------------+

+-----------------:+:----------------------------------------------------+
| \centering       | \vspace{-65pt}                                      |
| (line width=5pt) |   {\small                                           |
|                  | \begin{itemize}[label={}]                           |
|                  |     \item \verb|\begin{tikz-vowel}(line width=5pt)| |
|                  |     \item \verb|\end{tikz-vowel}|                   |
|                  | \end{itemize}                                       |
|                  |     }                                               |
+------------------+-----------------------------------------------------+

\bigskip
Examples of different colors, using the *color* option:

+----------------------------------:+:----------------------------------+
| \centering                        | \vspace{-100pt}                   |
|     {\charissil                   |   {\small                         |
|     \begin{tikz-vowel}(color=red) | \begin{itemize}[label={}]         |
|         \cardinalvowel[left]{i}{1 |     \item                         |
| }                                 |     \item \verb|\begin{tikz-vowel |
|         \cardinalvowel[right]{u}{ | }(color=red)|                     |
| v8}                               |         \begin{itemize}[label={}] |
|         \cardinalvowel{ə}{v11}    |             \item \verb|\cardinal |
|         \cardinalvowel{ɐ}{v15}    | vowel[left]{|{\charissil i}\verb| |
|     \end{tikz-vowel}              | }{1}|                             |
|     }                             |             \item \verb|\cardinal |
|                                   | vowel[right]{|{\charissil u}\verb |
|                                   | |}{v8}|                           |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ə}\verb|}{v11} |
|                                   | |                                 |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ɐ}\verb|}{v15} |
|                                   | |                                 |
|                                   |         \end{itemize}             |
|                                   |     \item \verb|\end{tikz-vowel}| |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

+----------------------------------:+:----------------------------------+
| \centering                        | \vspace{-100pt}                   |
|     {\charissil                   |   {\small                         |
|     \begin{tikz-vowel}(color=blue | \begin{itemize}[label={}]         |
| )                                 |     \item                         |
|         \cardinalvowel[left]{i}{1 |     \item \verb|\begin{tikz-vowel |
| }                                 | }(color=blue)|                    |
|         \cardinalvowel[right]{u}{ |         \begin{itemize}[label={}] |
| v8}                               |             \item \verb|\cardinal |
|         \cardinalvowel{ə}{v11}    | vowel[left]{|{\charissil i}\verb| |
|         \cardinalvowel{ɐ}{v15}    | }{1}|                             |
|     \end{tikz-vowel}              |             \item \verb|\cardinal |
|     }                             | vowel[right]{|{\charissil u}\verb |
|                                   | |}{v8}|                           |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ə}\verb|}{v11} |
|                                   | |                                 |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ɐ}\verb|}{v15} |
|                                   | |                                 |
|                                   |         \end{itemize}             |
|                                   |     \item \verb|\end{tikz-vowel}| |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

\bigskip
Examples of different text colors, using the *text* option:

+----------------------------------:+:----------------------------------+
| \centering                        | \vspace{-100pt}                   |
|     {\charissil                   |   {\small                         |
|     \begin{tikz-vowel}(text=red)  | \begin{itemize}[label={}]         |
|         \cardinalvowel[left]{i}{1 |     \item                         |
| }                                 |     \item \verb|\begin{tikz-vowel |
|         \cardinalvowel[right]{u}{ | }(text=red)|                      |
| v8}                               |         \begin{itemize}[label={}] |
|         \cardinalvowel{ə}{v11}    |             \item \verb|\cardinal |
|         \cardinalvowel{ɐ}{v15}    | vowel[left]{|{\charissil i}\verb| |
|     \end{tikz-vowel}              | }{1}|                             |
|     }                             |             \item \verb|\cardinal |
|                                   | vowel[right]{|{\charissil u}\verb |
|                                   | |}{v8}|                           |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ə}\verb|}{v11} |
|                                   | |                                 |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ɐ}\verb|}{v15} |
|                                   | |                                 |
|                                   |         \end{itemize}             |
|                                   |     \item \verb|\end{tikz-vowel}| |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

+----------------------------------:+:----------------------------------+
| \centering                        | \vspace{-100pt}                   |
|     {\charissil                   |   {\small                         |
|     \begin{tikz-vowel}(text=blue) | \begin{itemize}[label={}]         |
|         \cardinalvowel[left]{i}{1 |     \item                         |
| }                                 |     \item \verb|\begin{tikz-vowel |
|         \cardinalvowel[right]{u}{ | }(text=blue)|                     |
| v8}                               |         \begin{itemize}[label={}] |
|         \cardinalvowel{ə}{v11}    |             \item \verb|\cardinal |
|         \cardinalvowel{ɐ}{v15}    | vowel[left]{|{\charissil i}\verb| |
|     \end{tikz-vowel}              | }{1}|                             |
|     }                             |             \item \verb|\cardinal |
|                                   | vowel[right]{|{\charissil u}\verb |
|                                   | |}{v8}|                           |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ə}\verb|}{v11} |
|                                   | |                                 |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ɐ}\verb|}{v15} |
|                                   | |                                 |
|                                   |         \end{itemize}             |
|                                   |     \item \verb|\end{tikz-vowel}| |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

\bigskip
Examples of different line colors, using the *draw* option:

+----------------------------------:+:----------------------------------+
| \centering                        | \vspace{-100pt}                   |
|     {\charissil                   |   {\small                         |
|     \begin{tikz-vowel}(draw=red)  | \begin{itemize}[label={}]         |
|         \cardinalvowel[left]{i}{1 |     \item                         |
| }                                 |     \item \verb|\begin{tikz-vowel |
|         \cardinalvowel[right]{u}{ | }(draw=red)|                      |
| v8}                               |         \begin{itemize}[label={}] |
|         \cardinalvowel{ə}{v11}    |             \item \verb|\cardinal |
|         \cardinalvowel{ɐ}{v15}    | vowel[left]{|{\charissil i}\verb| |
|     \end{tikz-vowel}              | }{1}|                             |
|     }                             |             \item \verb|\cardinal |
|                                   | vowel[right]{|{\charissil u}\verb |
|                                   | |}{v8}|                           |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ə}\verb|}{v11} |
|                                   | |                                 |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ɐ}\verb|}{v15} |
|                                   | |                                 |
|                                   |         \end{itemize}             |
|                                   |     \item \verb|\end{tikz-vowel}| |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

+----------------------------------:+:----------------------------------+
| \centering                        | \vspace{-100pt}                   |
|     {\charissil                   |   {\small                         |
|     \begin{tikz-vowel}(draw=blue) | \begin{itemize}[label={}]         |
|         \cardinalvowel[left]{i}{1 |     \item                         |
| }                                 |     \item \verb|\begin{tikz-vowel |
|         \cardinalvowel[right]{u}{ | }(draw=blue)|                     |
| v8}                               |         \begin{itemize}[label={}] |
|         \cardinalvowel{ə}{v11}    |             \item \verb|\cardinal |
|         \cardinalvowel{ɐ}{v15}    | vowel[left]{|{\charissil i}\verb| |
|     \end{tikz-vowel}              | }{1}|                             |
|     }                             |             \item \verb|\cardinal |
|                                   | vowel[right]{|{\charissil u}\verb |
|                                   | |}{v8}|                           |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ə}\verb|}{v11} |
|                                   | |                                 |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ɐ}\verb|}{v15} |
|                                   | |                                 |
|                                   |         \end{itemize}             |
|                                   |     \item \verb|\end{tikz-vowel}| |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

Background transparency of labels in the `tikz-vowel` diagram {#sec:Background transparency of labels in the tikz-vowel diagram}
-------------------------------------------------------------

By default, the `tikz-vowel` diagram has no background. However, the
vowel labels have a white background. The reason for this is so that
they force a break in the diagram lines if they overlap. The problem
with this is that if you put the chart over a different color background
or an image then the white background of the vowel labels noticeably
contrasts with the background. To accommodate people who want/need the
break in the diagram lines *and* want the background of the vowel labels
to match the background, a transparency option has been created.

\bigskip
The example below has been placed on a red background to illustrate the
white background of the vowel labels:

+----------------------------------:+:----------------------------------+
| \centering                        |                                   |
|     {\charissil                   |                                   |
|     \begin{tikz-vowel}(show backg |                                   |
| round rectangle,background rectan |                                   |
| gle/.style={fill=red})            |                                   |
|         \cardinalvowel[left]{i}{1 |                                   |
| }                                 |                                   |
|         \cardinalvowel[right]{u}{ |                                   |
| v8}                               |                                   |
|         \cardinalvowel{ə}{v11}    |                                   |
|         \cardinalvowel{ɐ}{v15}    |                                   |
|     \end{tikz-vowel}              |                                   |
|     }                             |                                   |
+-----------------------------------+-----------------------------------+
| `background rectangle/.style={fil |                                   |
| l=red})`                          |                                   |
|                                   |                                   |
| -   `\cardinalvowel[left]{`i`}{1} |                                   |
| `                                 |                                   |
|                                   |                                   |
| -   `\cardinalvowel[right]{`u`}{v |                                   |
| 8}`                               |                                   |
|                                   |                                   |
| -   `\cardinalvowel{`ə`}{v11}`    |                                   |
|                                   |                                   |
| -   `\cardinalvowel{`ɐ`}{v15}`    |                                   |
|                                   |                                   |
| `\end{tikz-vowel}`                |                                   |
+-----------------------------------+-----------------------------------+

Using the transparency option eliminates these white backgrounds.
Depending on the PDF viewer you are using, it will look like those label
backgrounds have become the same as the diagram's background OR you will
see the same color, but (where applicable) the vowel label overlaps the
lines behind them.

+----------------------------------:+:----------------------------------+
| \centering                        |                                   |
|     {\charissil                   |                                   |
|     \begin{tikz-vowel}(show backg |                                   |
| round rectangle,background rectan |                                   |
| gle/.style={fill=red})<transparen |                                   |
| cy>                               |                                   |
|         \cardinalvowel[left]{i}{1 |                                   |
| }                                 |                                   |
|         \cardinalvowel[right]{u}{ |                                   |
| v8}                               |                                   |
|         \cardinalvowel{ə}{v11}    |                                   |
|         \cardinalvowel{ɐ}{v15}    |                                   |
|     \end{tikz-vowel}              |                                   |
|     }                             |                                   |
+-----------------------------------+-----------------------------------+
| `background rectangle/.style={fil |                                   |
| l=red})<transparency>`            |                                   |
|                                   |                                   |
| -   `\cardinalvowel[left]{`i`}{1} |                                   |
| `                                 |                                   |
|                                   |                                   |
| -   `\cardinalvowel[right]{`u`}{v |                                   |
| 8}`                               |                                   |
|                                   |                                   |
| -   `\cardinalvowel{`ə`}{v11}`    |                                   |
|                                   |                                   |
| -   `\cardinalvowel{`ɐ`}{v15}`    |                                   |
|                                   |                                   |
| `\end{tikz-vowel}`                |                                   |
+-----------------------------------+-----------------------------------+

The transparency option has a drawback, however. This drawback is the
reason this option is not set by default. Not all PDF viewers properly
support the `transparency group=knockout` option of a `tikzpicture`. The
`transparency group=knockout` option works in conjunction with the
`fill, opacity=0, text opacity=1` options of the vowel's label to create
a transparent background which 'sees' the document or `tikzpicture`
background while simultaneously erasing the other lines, nodes, and
labels behind it.

PDF viewers such as Adobe Acrobat displays these options correctly. Many
other PDF views, such as SumatraPDF, the default viewer for TeXworks,
and likely others, do not display this correctly.

The viewers which do not display it correctly will look like the vowel
label has *no* background. Therefore, if the vowel or diphthong label is
above a line, for example, they will overlap. The lines will not have a
nice break before the labels, which makes the diagram less attractive.

Vowels {#sec:Vowels}
======

The following tables present the trapezoidal vowel chart. The left chart
shows the location and number of cardinal[^1] vowels. The middle one
shows the actual cardinal vowels instead of the number. The right chart
shows a chart with all vowels at the cardinal vowel locations (including
both rounded and unrounded versions).

+:----------------------+:----------------------+:----------------------+
| \cardinalvowel{\footn | \charissil            | \charissil            |
| otesize 1}{1}         | \cardinalvowel{i}{1}  | \cardinalvowel[left]{ |
| \cardinalvowel{\footn | \cardinalvowel{e}{2}  | i}{1}                 |
| otesize 2}{2}         | \cardinalvowel{ɛ}{3}  | \cardinalvowel[right] |
| \cardinalvowel{\footn | \cardinalvowel{a}{4}  | {y}{1}                |
| otesize 3}{3}         | \cardinalvowel{ɑ}{5}  | \cardinalvowel[left]{ |
| \cardinalvowel{\footn | \cardinalvowel{ɔ}{6}  | e}{2}                 |
| otesize 4}{4}         | \cardinalvowel{o}{7}  | \cardinalvowel[right] |
| \cardinalvowel{\footn | \cardinalvowel{u}{8}  | {ø}{2}                |
| otesize 5}{5}         | \cardinalvowel{ɨ}{9}  | \cardinalvowel[left]{ |
| \cardinalvowel{\footn | \cardinalvowel{ɘ}{10} | ɛ}{3}                 |
| otesize 6}{6}         | \cardinalvowel{ə}{11} | \cardinalvowel[right] |
| \cardinalvowel{\footn | \cardinalvowel{ɜ}{12} | {œ}{3}                |
| otesize 7}{7}         | \cardinalvowel{ɪ}{13} | \cardinalvowel[left]{ |
| \cardinalvowel{\footn | \cardinalvowel{ʊ}{14} | a}{4}                 |
| otesize 8}{8}         | \cardinalvowel{ɐ}{15} | \cardinalvowel[right] |
| \cardinalvowel{\footn | \cardinalvowel{æ}{16} | {ɶ}{4}                |
| otesize 9}{9}         |                       | \cardinalvowel[left]{ |
| \cardinalvowel{\footn |                       | ɑ}{5}                 |
| otesize 10}{10}       |                       | \cardinalvowel[right] |
| \cardinalvowel{\footn |                       | {ɒ}{5}                |
| otesize 11}{11}       |                       | \cardinalvowel[left]{ |
| \cardinalvowel{\footn |                       | ʌ}{6}                 |
| otesize 12}{12}       |                       | \cardinalvowel[right] |
| \cardinalvowel{\footn |                       | {ɔ}{6}                |
| otesize 13}{13}       |                       | \cardinalvowel[left]{ |
| \cardinalvowel{\footn |                       | ɤ}{7}                 |
| otesize 14}{14}       |                       | \cardinalvowel[right] |
| \cardinalvowel{\footn |                       | {o}{7}                |
| otesize 15}{15}       |                       | \cardinalvowel[left]{ |
| \cardinalvowel{\footn |                       | ɯ}{8}                 |
| otesize 16}{16}       |                       | \cardinalvowel[right] |
|                       |                       | {u}{8}                |
|                       |                       | \cardinalvowel[left]{ |
|                       |                       | ɨ}{9}                 |
|                       |                       | \cardinalvowel[right] |
|                       |                       | {ʉ}{9}                |
|                       |                       | \cardinalvowel[left]{ |
|                       |                       | ɘ}{10}                |
|                       |                       | \cardinalvowel[right] |
|                       |                       | {ɵ}{10}               |
|                       |                       | \cardinalvowel{ə}{11} |
|                       |                       | \cardinalvowel[left]{ |
|                       |                       | ɜ}{12}                |
|                       |                       | \cardinalvowel[right] |
|                       |                       | {ɞ}{12}               |
|                       |                       | \cardinalvowel[left]{ |
|                       |                       | ɪ}{13}                |
|                       |                       | \cardinalvowel[right] |
|                       |                       | {ʏ}{13}               |
|                       |                       | \cardinalvowel{ʊ}{14} |
|                       |                       | \cardinalvowel{ɐ}{15} |
|                       |                       | \cardinalvowel[left]{ |
|                       |                       | æ}{16}                |
+-----------------------+-----------------------+-----------------------+

Cardinal^[\[footnote:cardinal vowel\]](#footnote:cardinal vowel){reference-type="ref" reference="footnote:cardinal vowel"}^ vowels {#sec:Cardinal vowels}
----------------------------------------------------------------------------------------------------------------------------------

\medskip
`\cardinalvowel[`*optional position*`]{`*required vowel
label*`}{`*required vowel number*`}`

Cardinal (and secondary) vowels are ones which fit into the slots 1 --
16, in the above diagram. To keep consistency and add flexibility, these
numbers (which are referred to as nodes) can be defined as either '1',
'2', ...'16' or 'v1', 'v2', ...'v16'. You can mix and match them also;
for example, you can call `\cardinalvowel` once with vowel node '5' and
call it a second time with vowel node '8'.

\bigskip
`[`*optional position*`]` options:

*left* \| *right* \| *above* \| *below* \| *above left* \| *above right*
\| *below left* \| *below right* \|

\bigskip
`{`*required vowel label*`}` options:

Can be almost anything without a `\par` or new line (e.g. `\\`).[^2]

\bigskip
`{`*required vowel number*`}` options:

*1*, *2*, ...*16* or *v1*, *v2*, ...*v16*

\bigskip
Examples with vowel number variation:

+-:+:-------------------------------------------------------------------+
|  | \vspace{-90pt}                                                     |
|  |   {\small                                                          |
|  | \begin{itemize}[label={}]                                          |
|  |     \item \verb|\begin{tikz-vowel}|                                |
|  |         \begin{itemize}[label={}]                                  |
|  |             \item \verb|\cardinalvowel{|{\charissil i}\verb|}{1}|  |
|  |             \item \verb|\cardinalvowel{|{\charissil e}\verb|}{2}|  |
|  |             \item \verb|\cardinalvowel{|{\charissil ɛ}\verb|}{3}|  |
|  |             \item \verb|\cardinalvowel{|{\charissil a}\verb|}{4}|  |
|  |             \item \verb|\cardinalvowel{|{\charissil æ}\verb|}{16}| |
|  |         \end{itemize}                                              |
|  |     \verb|\end{tikz-vowel}|                                        |
|  | \end{itemize}                                                      |
|  |     }                                                              |
+--+--------------------------------------------------------------------+

+-:+:-------------------------------------------------------------------+
|  | \vspace{-90pt}                                                     |
|  |   {\small                                                          |
|  | \begin{itemize}[label={}]                                          |
|  |     \item \verb|\begin{tikz-vowel}|                                |
|  |         \begin{itemize}[label={}]                                  |
|  |             \item \verb|\cardinalvowel{|{\charissil ɑ}\verb|}{v5}| |
|  |             \item \verb|\cardinalvowel{|{\charissil ɔ}\verb|}{v6}| |
|  |             \item \verb|\cardinalvowel{|{\charissil o}\verb|}{v7}| |
|  |             \item \verb|\cardinalvowel{|{\charissil u}\verb|}{v8}| |
|  |         \end{itemize}                                              |
|  |     \verb|\end{tikz-vowel}|                                        |
|  | \end{itemize}                                                      |
|  |     }                                                              |
+--+--------------------------------------------------------------------+

+-:+:--------------------------------------------------------------------+
|  | \vspace{-90pt}                                                      |
|  |   {\small                                                           |
|  | \begin{itemize}[label={}]                                           |
|  |     \item \verb|\begin{tikz-vowel}|                                 |
|  |         \begin{itemize}[label={}]                                   |
|  |             \item \verb|\cardinalvowel{|{\charissil ɨ}\verb|}{v9}|  |
|  |             \item \verb|\cardinalvowel{|{\charissil ɘ}\verb|}{10}|  |
|  |             \item \verb|\cardinalvowel{|{\charissil ə}\verb|}{v11}| |
|  |             \item \verb|\cardinalvowel{|{\charissil ɜ}\verb|}{12}|  |
|  |             \item \verb|\cardinalvowel{|{\charissil ɪ}\verb|}{v13}| |
|  |             \item \verb|\cardinalvowel{|{\charissil ʊ}\verb|}{14}|  |
|  |             \item \verb|\cardinalvowel{|{\charissil ɐ}\verb|}{v15}| |
|  |         \end{itemize}                                               |
|  |     \verb|\end{tikz-vowel}|                                         |
|  | \end{itemize}                                                       |
|  |     }                                                               |
+--+---------------------------------------------------------------------+

Examples with position specified:

+-:+:-------------------------------------------------------------------------+
|  | \vspace{-90pt}                                                           |
|  |   {\small                                                                |
|  | \begin{itemize}[label={}]                                                |
|  |     \item \verb|\begin{tikz-vowel}|                                      |
|  |         \begin{itemize}[label={}]                                        |
|  |             \item \verb|\cardinalvowel[left]{|{\charissil i}\verb|}{1}|  |
|  |             \item \verb|\cardinalvowel[left]{|{\charissil e}\verb|}{2}|  |
|  |             \item \verb|\cardinalvowel[left]{|{\charissil ɛ}\verb|}{3}|  |
|  |             \item \verb|\cardinalvowel[left]{|{\charissil a}\verb|}{4}|  |
|  |             \item \verb|\cardinalvowel[right]{|{\charissil ɑ}\verb|}{5}| |
|  |             \item \verb|\cardinalvowel[right]{|{\charissil ɔ}\verb|}{6}| |
|  |             \item \verb|\cardinalvowel[right]{|{\charissil o}\verb|}{7}| |
|  |             \item \verb|\cardinalvowel[right]{|{\charissil u}\verb|}{8}| |
|  |             \item \verb|\cardinalvowel{|{\charissil ə}\verb|}{11}|       |
|  |         \end{itemize}                                                    |
|  |     \verb|\end{tikz-vowel}|                                              |
|  | \end{itemize}                                                            |
|  |     }                                                                    |
+--+--------------------------------------------------------------------------+

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above]{|{\charissil ɨ}\verb |
|                                   | |}{9}|                            |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[below left]{|{\charissil ɘ} |
|                                   | \verb|}{10}|                      |
|                                   |             \item \verb|\cardinal |
|                                   | vowel{|{\charissil ə}\verb|}{11}| |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[below right]{|{\charissil ɜ |
|                                   | }\verb|}{12}|                     |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above left]{|{\charissil ɪ} |
|                                   | \verb|}{13}|                      |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above right]{|{\charissil ʊ |
|                                   | }\verb|}{14}|                     |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[below]{|{\charissil ɐ}\verb |
|                                   | |}{15}|                           |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

Coordinate (X, Y) and anywhere vowels {#sec:Coordinate (X, Y) and anywhere vowels}
-------------------------------------

In addition to cardinal vowels, it is possible to specify an arbitrary
location for a vowel. The vowel diagram is on a 4 (width) by 3 (height)
grid space. It is possible to go beyond this area (e.g. negative values
or above 4 or 3, respectively), but doing so will skew the diagram.

Two commands have been created where are nearly identical for this
purpose. The first, `\xyvowel`, allows you to specify using (X, Y)
notation. The second, `\anyvowel`, requires you send X as one argument
and Y as a different argument.

\medskip
`\xyvowel[`*optional position*`]`\
`{`*required vowel label*`}`\
`(`*required X,Y coordinates*`)`

`\anyvowel[`*optional position*`]`\
`{`*required vowel label*`}`\
`{`*required X coordinate*`}`\
`{`*required Y coordinate*`}`

The example below illustrates the `\xyvowel`:

+-:+:------------------------------------------------------------+
|  | \vspace{-90pt}                                              |
|  |   {\small                                                   |
|  | \begin{itemize}[label={}]                                   |
|  |     \item \verb|\begin{tikz-vowel}|                         |
|  |         \begin{itemize}[label={}]                           |
|  |             \item \verb|\xyvowel[above]{1}(0.25,2.25)|      |
|  |             \item \verb|\xyvowel[below left]{2}(3.25,1.75)| |
|  |             \item \verb|\xyvowel{3}(1.75,0.75)|             |
|  |         \end{itemize}                                       |
|  |     \verb|\end{tikz-vowel}|                                 |
|  | \end{itemize}                                               |
|  |     }                                                       |
+--+-------------------------------------------------------------+

\bigskip
The example below illustrates the `\anyvowel`:

+-:+:--------------------------------------------------------------+
|  | \vspace{-90pt}                                                |
|  |   {\small                                                     |
|  | \begin{itemize}[label={}]                                     |
|  |     \item \verb|\begin{tikz-vowel}|                           |
|  |         \begin{itemize}[label={}]                             |
|  |             \item \verb|\anyvowel[above]{1}{0.25}{2.25}|      |
|  |             \item \verb|\anyvowel[below left]{2}{3.25}{1.75}| |
|  |             \item \verb|\anyvowel{3}{1.75}{0.75}|             |
|  |         \end{itemize}                                         |
|  |     \verb|\end{tikz-vowel}|                                   |
|  | \end{itemize}                                                 |
|  |     }                                                         |
+--+---------------------------------------------------------------+

Diphthongs {#sec:Diphthongs}
==========

Diphthongs are vowels produced while the articulators are moving from an
starting point to an ending point. An arrow on the diagram is an easy
visual cue of this movement, just as `pst-vowel` does. The commands
defined in `tikz-vowel` accomplish the same basic task as `pst-vowel`
but provides seven commands to offer flexibility.

Except for the cardinal vowel to cardinal vowel diphthong, the other
diphthong commands have two versions which mirror the input of X,Y
coordinates seen in `\xyvowel` and `\anyvowel`. This difference is
purely for stylistic preference.

For all diphthong commands, the label is optional. This contrasts with
the vowel commands which have a mandatory label.

For all diphthong commands, the bend directions can be *left* or
*right*: default is *left*.

For all diphthong commands, the degree of curve/bend can be any number,
but a positive number between *0* and *90* will give the best results. A
negative number effectively flips the bend direction from *left* to
*right*, or vice versa. Numbers above *90* make curves which seem
unnatural in a vowel diagram setting. A curve/bend of *0* creates a
straight line.

Diphthong from cardinal vowel to cardinal vowel {#sec:Diphthong from cardinal vowel to cardinal vowel}
-----------------------------------------------

The command to draw a diphthong line from one cardinal vowel to another
cardinal vowel is `\cardinaldiphthong`:

\medskip
`\cardinaldiphthong[`*optional bend direction*`]`\
`{`*required starting vowel number*`}`\
`[`*optional degree of line curve/bend*`]`\
`{`*required ending vowel number*`}`\
`[`*optional vowel label*`]`\

The example below illustrates the `\cardinaldiphthong`:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{v1}{v8}|                |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong[right]{v1}{v16}|        |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{5}{11}[|{\charissil ɑə} |
|                                   | \verb|]|                          |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong[left]{13}{v15}[|{\chari |
|                                   | ssil ɪɐ}\verb|]|                  |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong[right]{v5}[90]{v8}[|{\c |
|                                   | harissil ɑu}\verb|]|              |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{6}[0]{v9}[|{\charissil  |
|                                   | ɔɨ}\verb|]|                       |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

Diphthong from cardinal vowel to (X, Y) / anywhere vowel {#sec:Diphthong from cardinal vowel to (X, Y) / anywhere vowel}
--------------------------------------------------------

The command to draw a diphthong line from one cardinal vowel to an (X,
Y) coordinate is `\cardinaltoxydiphthong`:

\medskip
`\cardinaltoxydiphthong[`*optional bend direction*`]`\
`{`*required starting vowel number*`}`\
`[`*optional degree of line curve/bend*`]`\
`(`*required X,Y coordinates*`)`\
`[`*optional vowel label*`]`\

The example below illustrates the `\cardinaltoxydiphthong`:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{v1}(3.75, 3.25)|        |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong[right]{v1}(1,2)|        |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{5}(2.25,1.75)[|{\charis |
|                                   | sil ɑ?}\verb|]|                   |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong[left]{13}(1.5,1.15)[|{\ |
|                                   | charissil ɪ?}\verb|]|             |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong[right]{v5}[90](4,2.5)[| |
|                                   | {\charissil ɑ?}\verb|]|           |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{6}[0](2.25,2.8)[|{\char |
|                                   | issil ɔ?}\verb|]|                 |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

The command to draw a diphthong line from one cardinal vowel to an X
coordinate and a Y coordinate is `\cardinaltoanydiphthong`:

\medskip
`\cardinaltoanydiphthong[`*optional bend direction*`]`\
`{`*required starting vowel number*`}`\
`[`*optional degree of line curve/bend*`]`\
`{`*required X coordinate*`}`\
`{`*required Y coordinate*`}`\
`[`*optional vowel label*`]`\

The example below illustrates the `\cardinaltoanydiphthong`:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\cardinal |
|                                   | toanydiphthong{v1}{3.75}{3.25}|   |
|                                   |             \item \verb|\cardinal |
|                                   | toanydiphthong[right]{v1}{1}{2}|  |
|                                   |             \item \verb|\cardinal |
|                                   | toanydiphthong{5}{2.25}{1.75}[|{\ |
|                                   | charissil ɑ?}\verb|]|             |
|                                   |             \item \verb|\cardinal |
|                                   | toanydiphthong[left]{13}{1.5}{1.1 |
|                                   | 5}[|{\charissil ɪ?}\verb|]|       |
|                                   |             \item \verb|\cardinal |
|                                   | toanydiphthong[right]{v5}[90]{4}{ |
|                                   | 2.5}[|{\charissil ɑ?}\verb|]|     |
|                                   |             \item \verb|\cardinal |
|                                   | toanydiphthong{6}[0]{2.25}{2.8}[| |
|                                   | {\charissil ɔ?}\verb|]|           |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

Diphthong from (X, Y) / anywhere vowel to cardinal vowel {#sec:Diphthong from (X, Y) / anywhere vowel to cardinal vowel}
--------------------------------------------------------

The command to draw a diphthong line from an (X, Y) coordinate to a
cardinal vowel is `\xytocardinaldiphthong`:

\medskip
`\xytocardinaldiphthong[`*optional bend direction*`]`\
`(`*required X,Y coordinates*`)`\
`[`*optional degree of line curve/bend*`]`\
`{`*required ending vowel number*`}`\
`[`*optional vowel label*`]`\

The example below illustrates the `\xytocardinaldiphthong`:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\xytocard |
|                                   | inaldiphthong[right](3.75, 3.25){ |
|                                   | v1}|                              |
|                                   |             \item \verb|\xytocard |
|                                   | inaldiphthong(1,2){v1}|           |
|                                   |             \item \verb|\xytocard |
|                                   | inaldiphthong(2.25,1.75){5}[|{\ch |
|                                   | arissil ?ɑ}\verb|]|               |
|                                   |             \item \verb|\xytocard |
|                                   | inaldiphthong[right](1.5,1.15){13 |
|                                   | }[|{\charissil ?ɪ}\verb|]|        |
|                                   |             \item \verb|\xytocard |
|                                   | inaldiphthong[left](4,2.5)[90]{v5 |
|                                   | }[|{\charissil ?ɑ}\verb|]|        |
|                                   |             \item \verb|\xytocard |
|                                   | inaldiphthong(2.25,2.8)[0]{6}[|{\ |
|                                   | charissil ?ɔ}\verb|]|             |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

The command to draw a diphthong line from an X coordinate and a Y
coordinate to a cardinal vowel is `\anytocardinaldiphthong`:

\medskip
`\anytocardinaldiphthong[`*optional bend direction*`]`\
`{`*required X coordinate*`}`\
`{`*required Y coordinate*`}`\
`[`*optional degree of line curve/bend*`]`\
`{`*required ending vowel number*`}`\
`[`*optional vowel label*`]`\

The example below illustrates the `\anytocardinaldiphthong`:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\anytocar |
|                                   | dinaldiphthong[right]{3.75}{3.25} |
|                                   | {v1}|                             |
|                                   |             \item \verb|\anytocar |
|                                   | dinaldiphthong{1}{2}{v1}|         |
|                                   |             \item \verb|\anytocar |
|                                   | dinaldiphthong{2.25}{1.75}{5}[|{\ |
|                                   | charissil ?ɑ}\verb|]|             |
|                                   |             \item \verb|\anytocar |
|                                   | dinaldiphthong[right]{1.5}{1.15}{ |
|                                   | 13}[|{\charissil ?ɪ}\verb|]|      |
|                                   |             \item \verb|\anytocar |
|                                   | dinaldiphthong[left]{4}{2.5}[90]{ |
|                                   | v5}[|{\charissil ?ɑ}\verb|]|      |
|                                   |             \item \verb|\anytocar |
|                                   | dinaldiphthong{2.25}{2.8}[0]{6}[| |
|                                   | {\charissil ?ɔ}\verb|]|           |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

Diphthong from (X, Y) / anywhere vowel to (X, Y) / anywhere vowel {#sec:Diphthong from (X, Y) / anywhere vowel to (X, Y) / anywhere vowel}
-----------------------------------------------------------------

The command to draw a diphthong line from an (X, Y) coordinate to
another (X, Y) coordinate is `\xydiphthong`:

\medskip
`\xydiphthong[`*optional bend direction*`]`\
`(`*required starting X,Y coordinates*`)`\
`[`*optional degree of line curve/bend*`]`\
`(`*required ending X,Y coordinates*`)`\
`[`*optional vowel label*`]`\

The example below illustrates the `\xydiphthong`:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\xydiphth |
|                                   | ong(0,3.25)(4, 3.25)|             |
|                                   |             \item \verb|\xydiphth |
|                                   | ong[left](0.25,2.75)[0](3.5,0.25) |
|                                   | |                                 |
|                                   |             \item \verb|\xydiphth |
|                                   | ong[right](3.85,0.15)(3.5,2.75)|  |
|                                   |             \item \verb|\xydiphth |
|                                   | ong[right](2.5, 0.5)[65](1,0.5)[| |
|                                   | {\charissil ??}\verb|]|           |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

The command to draw a diphthong line from an X coordinate and a Y
coordinate to a cardinal vowel is `\anydiphthong`:

\medskip
`\anydiphthong[`*optional bend direction*`]`\
`{`*required starting X coordinate*`}`\
`{`*required starting Y coordinate*`}`\
`[`*optional degree of line curve/bend*`]`\
`{`*required ending X coordinate*`}`\
`{`*required ending Y coordinate*`}`\
`[`*optional vowel label*`]`\

The example below illustrates the `\anydiphthong`:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\anydipht |
|                                   | hong{0}{3.25}{4}{3.25}|           |
|                                   |             \item \verb|\anydipht |
|                                   | hong[left]{0.25}{2.75}[0]{3.5}{0. |
|                                   | 25}|                              |
|                                   |             \item \verb|\anydipht |
|                                   | hong[right]{3.85}{0.15}{3.5}{2.75 |
|                                   | }|                                |
|                                   |             \item \verb|\anydipht |
|                                   | hong[right]{2.5}{0.5}[65]{1}{0.5} |
|                                   | [|{\charissil ??}\verb|]|         |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

Command for Ti*k*Z-related modifications {#sec:Command for TikZ-related modifications}
========================================

Using Ti*k*Z provides a huge amount of flexibility in the appearance of
the `tikz-vowel` chart. Some examples of this flexibility can be seen in
§[4.5](#sec:TikZ options for the tikz-vowel diagram){reference-type="ref"
reference="sec:TikZ options for the tikz-vowel diagram"}. Those options,
however, apply to the entire vowel chart and do not affect certain
options of the vowel nodes labels and the diphthong lines and labels.
For this reason, this package includes a bunch of commands which modify
specific options of the elements within the vowel chart. Using all of
these commands requires knowledge of the Ti*k*Z options for `\node`,
`\draw`, and the `label` option for each of these commands. Please refer
to the `tikz` manual for details.

Any Ti*k*Z option, any value {#sec:Any TikZ option, any value}
----------------------------

The following ten commands allow direct control over the vowel
marker/node, vowel label, diphthong line, and diphthong label (if
specified). Behind the scenes, these commands search for the specific
`\node` or `\draw` command specified and append the new option(s) and
option value(s). This done via regular expressions from the `l3regex`
package.

\bigskip
Two `\settikzvowelmarker` commands exist to modify a vowel marker/node's
appearance. The first allows you to specify a value-less `tikz` option
(e.g. `circle`) for a vowel marker/node. The second command requires
both a `tikz` option and a value (e.g. (option) `line width` and (value)
`5pt`) for a vowel marker/node.

l

 `\settikzvowelmarker{`*required vowel label*`}`\
`{`*required tikz property*`}`

\

l

 `\settikzvowelmarker{`*required vowel label*`}`\
`{`*required tikz property*`}`\
`{`*required tikz value*`}`

\

\bigskip
Two `\settikzvowellabel` commands exist to modify a vowel label's
appearance. The first allows you to specify a value-less `tikz` option
(e.g. `circle`) for a vowel label. The second command requires both a
`tikz` option and a value (e.g. (option) `line width` and (value) `5pt`)
for a vowel label.

l

 `\settikzvowellabel{`*required vowel label*`}`\
`{`*required tikz property*`}`

\

l

 `\settikzvowellabel{`*required vowel label*`}`\
`{`*required tikz property*`}`\
`{`*required tikz value*`}`

\

\bigskip
Four `\settikzdiphthong` commands exist to modify a diphthong line's
appearance. Using the diphthong label, the first allows you to specify a
value-less `tikz` option (e.g. `circle`) for a diphthong line. Using the
diphthong label, the second command requires both a `tikz` option and a
value (e.g. (option) `line width` and (value) `5pt`) for a diphthong
line.

The third `\settikzdiphthong` requires specifying the starting node
(either cardinal vowel or X,Y coordinates) and ending node (either
cardinal vowel or X,Y coordinates) and then specifying a value-less
`tikz` option (e.g. `circle`). The fourth command is the same but
requires an additional argument to assign the `tikz` option to.

If a diphthong label was specified during the creation of a diphthong,
you **must** use either the first or second command to modify a
diphthong. If the diphthong label was not specified when creating a
diphthong, you should use the third or fourth command.[^3]

l

 `\settikzdiphthong{`*required diphthong label*`}`\
`{`*required tikz property*`}`

\

l

 `\settikzdiphthong{`*required diphthong label*`}`\
`{`*required tikz property*`}`\
`{`*required tikz value*`}`

\

l

 `\settikzdiphthong{`*required starting vowel label (or 'X,Y'
coordinates)*`}`\
`{`*required ending vowel label (or 'X,Y' coordinates)*`}`\
`{`*required tikz property*`}`

\

l

 `\settikzdiphthong{`*required starting vowel label (or 'X,Y'
coordinates)*`}`\
`{`*required ending vowel label (or 'X,Y' coordinates)*`}`\
`{`*required tikz property*`}`\
`{`*required tikz value*`}`

\

\bigskip
Two `\settikzdiphthonglabel` commands exist to modify a diphthong
label's appearance. The first allows you to specify a value-less `tikz`
option (e.g. `circle`) for a diphthong label. The second command
requires both a `tikz` option and a value (e.g. (option) `line width`
and (value) `5pt`) for a diphthong label.

l

 `\settikzdiphthonglabel{`*required diphthong label*`}`\
`{`*required tikz property*`}`

\

l

 `\settikzdiphthonglabel{`*required diphthong label*`}`\
`{`*required tikz property*`}`\
`{`*required tikz value*`}`

\

### Multiple options, values - Tricks/hacks {#sec:Multiple options, values - Tricks/hacks}

If you abhor the idea of setting multiple options and values via
repeated calls to the `\settikz...{...}` commands above, there is a
trick to setting more at once. You can call the version of the command
with no value argument and put all of the Ti*k*Z options (and values, if
applicable) into the `{`*required tikz property*`}` argument. For
example, instead of calling `\settikzvowelmarker` twice to set the color
and then the line width, you could set both simultaneously via
`\settikzvowelmarker{draw=red,` `inner sep=5pt}{}`

\bigskip
The example below illustrates the trick:

+-----------------:+:-+
|                  |  |
+------------------+--+
| \hspace*{11.5em} |  |
| `inner sep=7pt}` |  |
+------------------+--+

Commands to change a specific option {#sec:Commands to change a specific option}
------------------------------------

The list of commands below will never be comprehensive due to the many
different options available in the `tikz` package and its many
libraries. However, there are some, such as setting the color or line
thickness, that are fairly common. If you are unfamiliar with Ti*k*Z or
simply want to use a command name that is descriptively helpful, these
commands are for you. More will be added if I get requests for something
very common.

There are already quite a lot of these 'wrapper' commands, and so I will
only present the commands and their options here. I will provide a
limited set of these in examples provided at the end of this section
(see
§[7.2.1](#sec:Examples of commands to change TikZ options){reference-type="ref"
reference="sec:Examples of commands to change TikZ options"}).

\bigskip
Two `\settikzvowelmarker` commands exist to modify a vowel marker/node's
appearance. The first allows you to specify a value-less `tikz` option
(e.g. `circle`) for a vowel marker/node. The second command requires
both a `tikz` option and a value (e.g. (option) `line width` and (value)
`5pt`) for a vowel marker/node.

l

 `\settikzvowelmarker{`*required vowel label*`}`\
`{`*required tikz property*`}`

\

### Examples of commands to change Ti*k*Z options {#sec:Examples of commands to change TikZ options}

This section contains a small assortment of the commands in
§[7.2](#sec:Commands to change a specific option){reference-type="ref"
reference="sec:Commands to change a specific option"} to provide a
flavor of what they look like and how they are used.

\bigskip
The example below illustrates the `\setdiphthonglabelcolor` command:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above]{i}{1}|               |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above right]{|{\charissil ɑ |
|                                   | }\verb|}{5}|                      |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{|{\charissil ɑ}\verb|}{ |
|                                   | |{\charissil ɑ}\verb|}[|{\chariss |
|                                   | il iɑ}\verb|]|                    |
|                                   |             \item \verb|\setdipht |
|                                   | honglabelcolor{|{\charissil iɑ}\v |
|                                   | erb|}{red}|                       |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

\bigskip
The example below illustrates the `\setdiphthonglabelbgcolor` command:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above]{i}{1}|               |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above right]{|{\charissil ɑ |
|                                   | }\verb|}{5}|                      |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{|{\charissil ɑ}\verb|}{ |
|                                   | |{\charissil ɑ}\verb|}[|{\chariss |
|                                   | il iɑ}\verb|]|                    |
|                                   |             \item \verb|\setdipht |
|                                   | honglabelbgcolor{|{\charissil iɑ} |
|                                   | \verb|}{red}|                     |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

\bigskip
The example below illustrates the `\setdiphthonglinecolor` command:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above]{i}{1}|               |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above right]{|{\charissil ɑ |
|                                   | }\verb|}{5}|                      |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{|{\charissil ɑ}\verb|}{ |
|                                   | |{\charissil ɑ}\verb|}[|{\chariss |
|                                   | il iɑ}\verb|]|                    |
|                                   |             \item \verb|\setdipht |
|                                   | honglinecolor{|{\charissil iɑ}\ve |
|                                   | rb|}{red}|                        |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

\bigskip
The example below illustrates the `\setdiphthonglabelcolor`,
`\setdiphthonglabelbgcolor`, and `\setdiphthonglinecolor` commands used
together:

+----------------------------------:+:----------------------------------+
|                                   | \vspace{-90pt}                    |
|                                   |   {\small                         |
|                                   | \begin{itemize}[label={}]         |
|                                   |     \item \verb|\begin{tikz-vowel |
|                                   | }|                                |
|                                   |         \begin{itemize}[label={}] |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above]{i}{1}|               |
|                                   |             \item \verb|\cardinal |
|                                   | vowel[above right]{|{\charissil ɑ |
|                                   | }\verb|}{5}|                      |
|                                   |             \item \verb|\cardinal |
|                                   | diphthong{|{\charissil ɑ}\verb|}{ |
|                                   | |{\charissil ɑ}\verb|}[|{\chariss |
|                                   | il iɑ}\verb|]|                    |
|                                   |             \item \verb|\setdipht |
|                                   | honglabelcolor{|{\charissil iɑ}\v |
|                                   | erb|}{red}|                       |
|                                   |             \item \verb|\setdipht |
|                                   | honglabelbgcolor{|{\charissil iɑ} |
|                                   | \verb|}{red}|                     |
|                                   |             \item \verb|\setdipht |
|                                   | honglinecolor{|{\charissil iɑ}\ve |
|                                   | rb|}{red}|                        |
|                                   |         \end{itemize}             |
|                                   |     \verb|\end{tikz-vowel}|       |
|                                   | \end{itemize}                     |
|                                   |     }                             |
+-----------------------------------+-----------------------------------+

Limitations {#sec:Limitations}
===========

Any bugs which may exist in the `tikz` package will exist here also. An
additional issue that arises not due to the `tikz-vowel` package or
`tikz` but due to PDF viewers is through the usage and proper viewing of
transparency (see the end of
§[4.6](#sec:Background transparency of labels in the tikz-vowel diagram){reference-type="ref"
reference="sec:Background transparency of labels in the tikz-vowel diagram"}).

The various commands defined within `tikz-vowel` also limit access to
various `tikz` functionality. This, in large part, was by design.
Modifiying the wrong settings or setting option to values that you do
not understand properly from `tikz` has the potential to break or
significantly alter the vowel diagram which `tikz-vowel` provides. If
you need or want more flexibility, it is suggested that you not use
`tikz-vowel` and manually create the diagram using `tikz` or another
method of your choosing.

Disclaimer {#sec:Disclaimer}
==========

There is no warranty for the `tikz-vowel` package and all files therein,
henceforce the Work. Except when otherwise stated in writing, the
Copyright Holder provides the Work 'as is', without warranty of any
kind, either expressed or implied, including, but not limited to, the
implied warranties of merchantability and fitness for a particular
purpose. The entire risk as to the quality and performance of the Work
is with you. Should the Work prove defective, you assume the cost of all
necessary servicing, repair, or correction.

In no event unless required by applicable law or agreed to in writing
will The Copyright Holder, or any author named in the components of the
Work, or any other party who may distribute and/or modify the Work as
permitted above, be liable to you for damages, including any general,
special, incidental or consequential damages arising out of any use of
the Work or out of inability to use the Work (including, but not limited
to, loss of data, data being rendered inaccurate, or losses sustained by
anyone as a result of any failure of the Work to operate with any other
programs), even if the Copyright Holder or said author or said other
party has been advised of the possibility of such damages.

While the author has taken many reasonable steps to ensure the package
is working as presented and the documentation aids in understanding and
accessing features of the package, errors or bugs may exist still. If
you choose to use this package, please make sure you have a backup
method to create vowel charts in the event that somethings does not work
as expected.

Finally, this package is provides access to and features of the `tikz`
package. If you choose to use the `tikz-vowel` package and its commands,
or `tikz` commands within `tikz-vowel`, it is your responsibility to
resolve the `tikz` issues or to consult its documentation. This
documentation is not intended to help with the understanding or usage of
`tikz` in any way.

Acknowledgements {#sec:Acknowledgements}
================

I would like to thank Rei Fukui for creating the original `vowel` and
Alan Munn for work on the `pst-vowel`, both packages upon which this
`tikz-vowel` package is based. I have based many parts of this
documentation on either one of those packages, in order to aid in
transitioning from either of them to this package.

It was via discussion with Alan that I realized a transparency issue
existed when using `pstricks` (and, therefore, `pst-vowel`) when using .
That discussion ultimately led me to creating this package.

In addition to the above package authors, I had a great deal of help
from numerous people online, either via accessible documents or often
from my own or previously posted questions on Stack Exchange. I would
have struggled or possibly never gotten this package completed without
such help!

[^1]: I use the term 'cardinal vowels' in this package to actually refer
    to both cardinal vowels and secondary vowels. This is in keeping
    with the way `vowel` package handled it and also to avoid two
    commands which are basically
    identical.[\[footnote:cardinal vowel\]]{#footnote:cardinal vowel
    label="footnote:cardinal vowel"}

[^2]: A label longer than one character will probably create overlapping
    issues. Vowel location option `[`*center*`]` (or an unspecified
    location) will never have a label overlap its dot, because there is
    no dot. Generally, `[`*above left*`]`, `[`*above*`]`, `[`*above
    right*`]`, `[`*right*`]`, `[`*below left*`]`, `[`*below*`]`, and
    `[`*below right*`]` will avoid the label overlapping its vowel dot.
    However, depending on various factors, there may be overlap between
    two vowels, a vowel and a diphthong, or two diphthongs. These
    overlaps can be avoided either by altering the order in which you
    create vowels or diphthongs *and/or* altering the location option of
    one or more vowel labels.

[^3]: Technically, you can use the first two commands, but you need to
    understand how the line name (i.e. the Ti*k*Z option `name path`)
    was created. The line name is set to the diphthong label, if
    specified. If not, the line name is set to the starting node plus
    'to' plus the ending node (minus the commas, if using X,Y
    coordinates). Depending on which of the diphthong commands was used
    (see §[6](#sec:Diphthongs){reference-type="ref"
    reference="sec:Diphthongs"}), will depend on the exact line name.
    For example, starting node is '1.2,3.4' and ending node is
    '2.75,0.5' will result in a line name of '1.23.4to2.750.5'. If the
    starding node is cardinal vowel 'v1' and ending node is '2.75,0.5',
    then the line name will be 'v1to2.750.5'. Etc.
