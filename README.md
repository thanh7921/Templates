# Templates file for various tools

The folder structure:

```
.
├── clang-format
│   ├── chromium_style
│   ├── custom_cpp_style
│   ├── gnu_style
│   ├── google_style
│   ├── llvm_style
│   ├── microsoft_style
│   ├── mozilla_style
│   └── webkit_style
├── cv
│   ├── cv.tex
│   ├── Makefile
│   └── portrait.jpg
├── image
│   └── bk_logo.png
├── license
│   └── mit
├── make
│   └── Makefile_C_C++
├── pandoc_markdown
│   ├── fancy_report
│   ├── simple_report
│   ├── slide
│   └── pandoc_markdown_syntax.md
└── README.md

```

## Clang format file

All the setting for clang-format (store in files) is stored here. My custom style (which I
personally like) is `custom_cpp_style`.

## CV 

Latex template for my CV. Invoke make to compile the final CV as pdf file.

## Image

I store images that I used often here

## License 

Commonly used license, contain MIT for now.

## Make

I also use makefile for various purpose, currently I have one for small to medium sized C/C++
project.

## Pandoc markdown

I use pandoc to convert all my report and slide at school to pdf. The `pandoc_markdown` folder
contains template for simple report, slide and fancy report.

