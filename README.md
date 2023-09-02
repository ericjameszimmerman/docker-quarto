# Quarto Docker

## Docker Build Commands
```bash

# build image
docker build -t quarto-alpha quarto-ubuntu

# create container
docker run --name quartotest -it quarto-alpha
```

## Quarto Render Commands
```bash
quarto render --to pdf --toc
quarto render --output-dir htmloutput --to html
```