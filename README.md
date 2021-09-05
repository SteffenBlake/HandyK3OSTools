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

# List of tools added

## Aliases

A full list can be found in the .aliases script file, but here are some of the important ones to make it easier to remember the naming conventions:

### ki/kc/kr/ku/kd = get / create / describe / patch / delete

I simply just remember things easier in the convention of ICRUD, "Index" / "Create" / "Read" / "Update" / "Delete", mostly because I am a web dev and thats more how I remember stuff.

I also explicitly added `--show-kind` to the `kubectl get` command for `ki` because it makes copy pasting way easier from the name column on the output to drill down and get data better. This small tweak was a pretty nice improvement to workflow imo

### cheatsheet

This simply just `cat`s out the contents of the .cheatsheet file, which is a handy reference for all the shorthands for default kubernetes objects.

### config

Another quick shorthand for quickly opening up the k3os persistant config file located at `/var/lib/rancher/k3os/config.yaml`. This file doesnt exist by default, but you can modify it to append more custom configurations to your k3os instance, and I often test my stuff out this way so having a fast way to pop it open was handy.

### logs

Most of the logs we tend to care about exist in `/var/log/*.log`, this command is a quick way to pop open a `tail -f` on all the logs there for on the fly watching whats actually happening right now

### everything

This is something I wish kubectl had by default and it was a bit of a pain hunting it down, but its *exactly* what I needed. This gets **EVERYTHING** for your specific query. Usually you will use this with `-n <namespace>` on the end.

Because turns out `kuebctl get all -A` doesnt actually get everything, and sometimes you really do just want EVERYTHING

### apply/create/delete

Shorthands for `kubectl <action> -f` because I just got so goddamn tired of forgetting the -f all the time, and the vast majority of my Apply/Deletes were on yaml files I had, so I wanted quick shorthands for this

## chart

Usage: `chart <name>`
Requirements: you have a HelmChart and HelmChartConfig yaml file located at `/home/<name>/<name>.yaml` and `/home/<name>/<name>-config.yaml` respectively

Will automatically create the namespace `<name>-system` and copy those two yaml files over to `/var/lib/rancher/k3s/server/manifests` which is the mechanism k3os uses for deploying helm.

See more details on how all that stuff works here: https://rancher.com/docs/k3s/latest/en/helm/

## unchart

Usage: `unchart <name>`

Will delete the namespace, HelmChart, and HelmChartConfigs, as well as the two associated yaml files located in `/var/lib/rancher/k3s/server/manifests`, see the details for `chart` above. Basically its the "undo" for the above command.

## finalize

Usage: `finalize <kubernetes object>`

A quick shorthand for patching an object's finalizers to be empty. Useful for cleaning up those objects that refuse to actually close up because of hanging finalizers.

Example: `finalize pods/hanging-pod-0 -n hung-namespace`

## helm

We dont have *actual* helm inside of k3os, but we can do the weird rancher thing to deploy helm charts. See:  https://rancher.com/docs/k3s/latest/en/helm/

Usage: `helm <repo> <chart>`
Example: `helm https://charts.gitlab.io/ gitlab`

This doesnt actually do the deploy yet, but what it does do is pre-compose `<chart>.yaml` and `<chart>-config.yaml` inside of `/home/<chart>`

IE the above command would create the two files `/home/gitlab/gitlab.yaml` and `/home/gitlab/gitlab-config.yaml` which will adhere to the specs outlined in the rancher documenation linked above.

You can then, after editting those files, utilize `chart` command to actually deploy it.

## namespace

Quick shorthand for changing the namespace of your current context, so you dont have to keep `-n`ing all your commands.
