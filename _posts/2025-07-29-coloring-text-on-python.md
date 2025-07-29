---
title: "Color your printing text for Jupyter/Terminal via Python"
date: 2025-06-29 15:05 -0900
categories: python, jupyter
---

# Color your printing text for Jupyter/Terminal via Python

```Python

font_codes = {
    # color
    "black": '\033[30m',
    "red": '\033[31m',
    "green": '\033[32m',
    "yellow": '\033[33m',
    "blue": '\033[34m',
    "magenta": '\033[35m',
    "cyan": '\033[36m',
    "white": '\033[37m'
}
bg_codes = {
    # bg
    "black": '\033[40m',
    "red": '\033[41m',
    "green": '\033[42m',
    "yellow": '\033[43m',
    "blue": '\033[44m',
    "magenta": '\033[45m',
    "cyan": '\033[46m',
    "white": '\033[47m'
}
sty_codes = {
    "bold": "\033[1m",
    "italic": "\033[3m",
    "underline": "\033[4m",
}


def print_color(text: str, color: str = 'black', bg: str = 'none', bold: bool = False, italic: bool = False, underline: bool = False):
    font_color = font_codes[color] if color in font_codes else ""
    bg_color = bg_codes[bg] if bg in bg_codes else ""
    style = (sty_codes['bold'] if bold else '') + (sty_codes['italic'] if italic else '') + (sty_codes['underline'] if underline else '')
    ending = "\033[0m" if (len(font_color) + len(bg_color) + len(style)) > 0 else ""
    return font_color + bg_color + style + text + ending


# try:
print(print_color("your text", color="green", bold=True, bg="yellow"))
```
