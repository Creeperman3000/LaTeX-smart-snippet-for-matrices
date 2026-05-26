# LaTeX-smart-snippet-for-matrices
This snippet allows users to insert [matrices](https://en.wikipedia.org/wiki/Matrix_(mathematics)) with custom surrounding characters, dimensions and alignments using [UltiSnips](https://github.com/SirVer/ultisnips).

Type `Trigger` + `"mat"` + \<Tab\> (or your `g:UltiSnipsExpandTrigger`) to insert a matrix.

### Delimiters

With the exeption of array, you can think of this as putting the `Trigger` before the word matrix like so:

```latex
\begin{<Trigger>matrix}
…
\end{<Trigger>matrix}
```

If no `<Trigger>` char is provided, an array environment will be used.

```latex
\begin{array}
…
\end{array}
```

| Trigger        | Delimiter     |
|----------------|---------------|
| `pmat`+\<Tab\> | `(…)`         |
| `bmat`+\<Tab\> | `[…]`         |
| `Bmat`+\<Tab\> | `{…}`         |
| `vmat`+\<Tab\> | `\|…\|`       |
| `Vmat`+\<Tab\> | `\|\|…\|\|`   |
| `mat`+\<Tab\>  | no delimiters |

### Size

Append `{cols}x{rows}` **before** pressing Tab:

```latex
pmat3x2+<Tab> →
\begin{array}
A & B \\
C & D \\
E & F
\end{array}
```

- where both {cols} and {rows} must be positive integers
- default values are 2x2
- Cells are labelled A–Z (looped). Replace them with your content after expansion.
- Snippet trigger uses `r` (Tab‑expand) to allow you to type the size and alignment before expanding.

### Alignment

- Alignment chars `c`, `l`, `r`, `S` (from [siunitx](https://ctan.org/pkg/siunitx)) can be appended afterward.
- `|` can also be placed between alignment characters for a vertical bar.

Environments from [asmmath](https://ctan.org/pkg/amsmath) (`pmatrix`, `bmatrix`, …) do not support alignment characters.
This means that when alignment **is** given, `array` + `\left`/`\right` is used instead.

**Alignment chars logic:**

- If no alignment chars is provided, a `c` (center) character is used if necessary.
- If only one alignment chars is provided, it will be used for all cols.
- If more than one alignment char is used, they will be used according to this logic:
  1. Fist char is for the first row.
  2. Last char is for the last row.
  3. Char in between is used for all remaining cols (it might be dropped if the matrix has only 2 cols).
  4. If more than one char is in between, they will be written multiple times (from left to right), until alignment for all cols is specified.
- If there are more alignment chars than cols, the chars are dropped in reverse order.

**Vertical bar placement logic:**

| snippet trigger        | alignment                         |
|------------------------|-----------------------------------|
|`pmat4x1lcr`            | `…{lccr}…`                        |
|`pmat4x1l\|cr`          | `…{l\|ccr}…`                      |
|`pmat4x1lc\|r`          | `…{lcc\|r}…`                      |
|`pmat4x1l\|c\|r`        | `…{l\|cc\|r}…`, unless specified: |
|`pmat4x1l\|c\|c\|r`     | `…{l\|c\|c\|r}…`                  |
|`pmat4x1\|l\|c\|c\|r\|` | `…{\|l\|c\|c\|r\|}…`              |

---

I would recomend adding this as your preamble. Full credit to u [@gillescastel](https://github.com/gillescastel/latex-snippets), licensed under the MIT License.

```python
global !p
def math():
	return vim.eval('vimtex#syntax#in_mathzone()') == '1'
def comment(): 
	return vim.eval('vimtex#syntax#in_comment()') == '1'
def env(name):
	[x,y] = vim.eval("vimtex#env#is_inside('" + name + "')") 
	return x != '0' and y != '0'
endglobal
```
