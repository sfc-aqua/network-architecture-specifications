# network-architecture-specifications
RFC specification for network architecture

## How to setup the toolchains

For this setup, we assume that you have a working ruby installation and a python installation.
If you are a mac user, we recommend using homebrew to install the latest version of ruby and python but the stock version should suffice.

### Installation 

To install [kramdown-rfc](https://github.com/cabo/kramdown-rfc), 
```bash
$ gem install kramdown-rfc
```

[kramdown-rfc](https://github.com/cabo/kramdown-rfc) can only compiles the markdown flavour of RFC into XML. 
To see the output in other format, `txt` or `pdf`, you need another tool like [xml2rfc](https://github.com/ietf-tools/xml2rfc#installation).

To install [xml2rfc](https://github.com/ietf-tools/xml2rfc#installation), we recommend using python `pip`. 
If you don't want to pollute your global space, we recommend making virtual environment for it.
You can learn more [here](https://docs.python.org/3/library/venv.html#creating-virtual-environments). Or we recommend trying out [pyenv](https://github.com/pyenv/pyenv)
```bash
$ pip install xml2rfc
```

Then in order to confirm the installation, you can try running `kdrfc` command, this command compiles the markdown file into `.xml` and into `.txt.` in one go.
Basically it runs `kramdown-rfc` followed by `xml2rfc` in the background.
```bash
$ kdrfc test.md
```
If you see the txt output created and message in the terminal then it is working properly and you are good to go.