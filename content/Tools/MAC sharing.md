
# brew & brew cask & Cellar & formula & tap
1. brew download source code, build and install, mostly for programmer; brew cask is an extension to brew, mostly for normal user and GUI applications.
2. brew cask syntax is deprecated. use `brew <cmd> --cask` instead.
3. brew install things to **Cellar** dir `/usr/local/Cellar` on Intel CPU and `/opt/homebrew/Cellar` on Apple Silicon.
4. **formula** is the package definition for brew, **casks** is the one for Cask
5. **tap**(Third Party Repository) is the source of formula, brew tap adds more repos to the list of formula that brew needs
