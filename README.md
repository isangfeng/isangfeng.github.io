# Introduction

This is a repository of my homepage. There are some points for preview and deploy it.

# Usage

To preview using this command.

```bash
quarto preview
```

To generate html files for deploying.

```bash
quarto render # convert qmd file to md file.
hugo # convert md file to html file.
# hugo server # will generate html with localhost.
```

Commit and sync into github with docs folder for github's action.

```bash
git add ...
git commit -m "xxx"
git push origin main
```

## Update: 2026.05.30

I have added quarto into github action workflow. So, it's not necessary to render qmd file to md file and convert md file to html file. The only thing need to do is to write in md files.<D-s>


