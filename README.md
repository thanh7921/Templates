# Templates file for various tools

The folder structure:

```
.
├── make
│   └── Makefile_C_C++
├── pandoc_markdown
│   ├── pandoc_markdown_syntax.md
│   ├── simple_report
│   │   ├── build_config.yaml
│   │   ├── content.md
│   │   ├── Makefile
│   │   └── metadata.yaml
│   └── slide
│       ├── build_config.yaml
│       ├── content.md
│       ├── Makefile
│       └── metadata.yaml
└── README.md
```

## Pandoc markdown

I use pandoc to convert all my report and slide at school to pdf. The `pandoc_markdown` folder
contains just that - a template for simple report and a template for a slide.

## Make

I also use makefile for various purpose, currently I have one for small to medium sized C/C++
project.
