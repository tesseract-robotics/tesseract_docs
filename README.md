# tesseract_docs

## Build Documentation Locally 

1. Install
   - `sudo apt install python3-sphinx python3-git python3-recommonmark`
   - `pip3 install sphinxext-remoteliteralinclude sphinxcontrib-spelling`
1. Build
   - `sphinx-build -b html tesseract_docs builddir`
