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
├── image
│   └── bk_logo.png
├── make
│   └── Makefile_C_C++
├── pandoc_markdown
│   ├── pandoc_markdown_syntax.md
│   ├── simple_report
│   └── slide
└── README.md

7 directories, 12 files
```

## Pandoc markdown

I use pandoc to convert all my report and slide at school to pdf. The `pandoc_markdown` folder
contains just that - a template for simple report and a template for a slide.

## Make

I also use makefile for various purpose, currently I have one for small to medium sized C/C++
project.

## Image

I store images that I used often here

## Clang format file

All the setting for clang-format (store in files) is stored here. My custom style (which I
personally like) is `custom_cpp_style`.
