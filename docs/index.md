# Welcome

[Material Theme](https://squidfunk.github.io/mkdocs-material/) for [MkDocs](https://www.mkdocs.org).

### Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

### Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

---

```sh
cd /var/www/html/mkdocs/

python3 -m venv .venv 
# this creates a hidden directory named .venv inside
# your project folder containing a isolated copy of the Python binary and pip.
source venv/bin/activate

mkdocs serve # Live update server at http://127.0.0.1:8000/mkdocs/site/
mkdocs build

deactivate # exit the virtual environment (venv)

```
