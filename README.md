# HandyK3OSTools
A bunch of handy scripts and tools for k3os bootstrapping download

# How to import and test

Simply execute the two following commands within k3os as the Rancher user (or whatever else your root user may be)

## Warning: This will overwrite your ~/.profile file if you have a custom one made!!!
```
source <(curl -s https://raw.githubusercontent.com/SteffenBlake/HandyK3OSTools/main/Scripts/.get-tools)
get-tools
```

This will download all of the script files located in `<this repo>/Scripts/` to your users `~/` location and overwrite the `.profile` to include them all as sources.

# Updating to use the latest version of the tools

Simply just re-invoke `get-tools` again and it will redownload the latest scripts from this repo. Keep in mind everytime you invoke it, `~/.profile` will get overwritten!
