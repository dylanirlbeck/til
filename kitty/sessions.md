# Kitty Sessions

This TIL ended up being longer than I had anticipated, so I turned it into
a [blog post](https://dev.to/dylanirlbeck/kitty-sessions-44j2)!

---

One of the best features about Kitty are [startup
sessions](https://sw.kovidgoyal.net/kitty/index.html#startup-sessions). Sessions
allow you to create one or more tabs on startup (or with the `kitty --session`
command) and customize each tab with a unique terminal configuration. For
example, my current `startup.conf` looks like this:

```conf
# startup.conf
new_tab tailwind_ppx
cd ~/Code/reason/tailwind_ppx
title vim
launch zsh -c 'nvim'
launch zsh -c 'esy watch'
launch zsh
enabled_layouts tall:bias=50;full_size=1
layout tall

new_tab other
cd ~
launch zsh
enabled_layouts tall:bias=50;full_size=1
layout tall
```

which sets up two
[tabs](https://sw.kovidgoyal.net/kitty/index.html#tabs-and-windows) when
I launch Kitty: the first is a Reason native project (in particular,
[`tailwind-ppx`](https://github.com/dylanirlbeck/tailwind-ppx)) and the second
is a new tab that I can use for whatever I need to. This configuration looks
like this:

<img height="500" src="/assets/kitty/startup.png" />

## "Dynamic" Sessions

Normal startup sessions are great if you've hard-coded the desired working
directory for the session into the `.conf` file, but what about when you want to
start a session and you don't know the directory? With a little help from `zsh`,
we can implement what I'm calling "dynamic" Kitty sessions. Essentially, we're
going to create a command that allows you to launch a new Kitty session given
the path to a project. For example,

````
$ kt-native ~/tailwind_ppx # Launch a Reason native session
$ kt-js ~/some_js_project # Launch a JavaScript session ```
````

As you can start to see, there's a lot of customization we can do in the middle,
but hopefully you get the basics of what we're trying to do.

### Defining our session configurations

Let's start by defining our session. Personally, [I've
created](https://github.com/dylanirlbeck/dotfiles/tree/master/config/kitty) one
session file per type of project I work with (Reason native, BuckleScript,
JavaScript, etc.), and it's worked well for me thus far.

> Note that you can put your session files wherever you'd like, so long as you
> properly provide the path in your .zshrc

For example, here's my session for a Reason native project:

```
// ~/path/to/reason_native.conf

new_tab reason_native
cd ${PROJECT_DIR}
title vim launch zsh -c 'nvim'
launch zsh -c 'esy watch'
launch zsh
enabled_layouts tall:bias=50;full_size=1
layout tall
```

> Notice the `PROJECT_DIR` environment variable above --- this variable will be
> set automatically before Kitty is launched, so stay tuned.

### Creating launch aliases

Now that we've created the session configuration files, we'll move on to
creating shortcuts in `zsh` so instead of doing this:

```
$ export PROJECT_DIR=/path/to/reason_native_project
$ kitty --session ~/path/to/reason_native.conf
```

we can just do

```
$ kt-native /path/to/reason_native_project
```

Since these shortcuts will take in an argument (namely, the path to a project),
all we need to do is add a function in our `.zshrc` that will act as our
alias.

```
// ~/path/to/.zshrc

# Kitty functions
function kt-native() {
  export PROJECT_DIR=$1
  kitty --session ~/path/to/reason_native.conf
}
```

And that's it! Now you can launch a new Kitty session based on the type of
project, without hard-coding all your projects into the `.conf` files
beforehand.
