# Central Docs 
The goal of Central Docs is to to establish a single, authoritative source for all policies, procedures, and practices across all 
city repositories and development projects.

These guidelines are intended to facilitate, rather than restrict, the collaborative development experience, and we encourage all 
involved to discuss, contribute, and improve this documentation.

## Viewing the published documentation
The Central Docs GitHub Pages site is available at [https://city-of-saint-louis.github.io/central-docs/](https://city-of-saint-louis.github.io/central-docs/) 

## Viewing the documentation locally 
MkDocs can generate a local live preview of the documentation that is useful when updating or adding content. In order to use this 
feature:
1. Clone the repo and cd to the directory where you've cloned it. 
2. Install locally (preferably from a virtual environment): `pip install -r requirements.txt`
3. Launch the MkDocs preview: `mkdocs serve`

## Building the documentation locally 
MkDocs can also generate static HTML using the command `mkdocs build`. 
The default output path for this operation is `docs/site/`.

### About 
This project was built with [MkDocs](https://www.mkdocs.org/) and uses the [Material theme](https://github.com/squidfunk/mkdocs-material). 