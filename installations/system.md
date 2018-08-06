## Mac

### Uninstall python 3.7 and install python 3.6

```
# see which version is python currently pointing to
brew link python --dry-run
# remove that version(in my case it was python 3.7 and I needed to install 3.6)
brew unlink python
# find specific version to install, this is brew formula for python 3.6
brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/f2a764ef944b1080be64bd88dca9a1d80130c558/Formula/python.rb
```
