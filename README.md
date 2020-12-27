## First steps on Apple silicon powered system

This repo includes snippets and helpful things to use a Apple silicon Mac. 

### Running terminal commands through Rosetta 2

This is specially useful to run shell scripts where there is no execution
binary to tell "this is the arch you should be using."

One can invoke any command in bash through Rosetta 2 with the `arch` command to invoke bash as it was running in an x86 system:
```
$ arch -x86_64 /bin/bash "echo this is running in through Rosetta 2"
```

### Installing Brew with Rosetta 2

We prepend `arch` to the usual call to install brew

```
$ arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Setting up a terminal profiles that will run everything through Rosetta 2

- Take your favorite profile in Terminal and duplicate it. Open the profiles 
tab in the  preferences menu in the terminal app, select your profile, click on
the "three dots in a circle" below, and click `Duplicate profile`. Then name it
as you prefer, in my case `Pro Rosetta`.

![alt text](duplicate.png)

- Edit your new profile to run its shell with Rosetta 2. In the same profiles
tab as above, select the new profile (`Pro Rosetta` in the example), select
the "Shell" tab, check "Run command", and add:
```
env /usr/bin/arch -x86_64 /bin/bash --login
```
I use bash as my default, shell, but if you use `zsh` you would add the
following instead:
```
env /usr/bin/arch -x86_64 /bin/zsh --login
```
![alt text](edit_profile.png)

### Working with golang

There are two options to install go's compiler:

- Install the x86 golang compiler, let Rosetta 2 take over, when running the
compiler and producing binaries targeting x86 arch.
- Install the `macOS ARMv8` version of 1.16 beta. It will run natively and
produce binaries that will run without involving Rosetta 2.

For compiling, you can target an specific OS and architecture (instructions
copied from [Digitalocean](https://www.digitalocean.com/community/tutorials/how-to-build-go-executables-for-multiple-platforms-on-ubuntu-16-04):

```
$ env GOOS=target-OS GOARCH=target-architecture go build package-import-path
```

These are some potentially interesting targets:

- *Mac with x86-64*:
```
$ env GOOS=darwin GOARCH=amd64 go build package-import-path
```

- *Mac with Apple silicon* (not sure if this target is available
if you a running a version previous to golang 1.16):
```
$ env GOOS=darwin GOARCH=arm64 go build package-import-path
```

- *Linux system with x86-64*:
```
$ env GOOS=linux GOARCH=amd64 go build package-import-path
```

