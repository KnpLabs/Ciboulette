# Ciboulette, your local CI server

__NOTE__ This is alpha software, it might very well completely mess with your git hooks. Use with caution!

## What it is

Ciboulette is a local, unobtrusive CI server. It is designed to run as a git `post-commit` hook.

![Ciboulette](http://f.cl.ly/items/0P3a3I2f241C0n3H3O3F/Screen%20Shot%202012-03-10%20at%201.22.05%20AM.png)

## Installation

1. clone the repository
2. copy `bin` and `lib` anywhere in your `$PATH`
3. `cd` to the repository you want to CI
4. run `ciboulette --install`

## Configuration

Ciboulette is configured via git's configuration. The only mandatory option is `ciboulette.runner`, which is the script that will be used to build your project.

You can either set it by hand:

```
git config --add ciboulette.runner "your_script"
```

or use ciboulette's interactive configurator:

```
ciboulette --config
```

## Notifiers

Right now, ciboulette only supports Growl notifications through [growlnotify](http://growl.info/downloads#generaldownloads). You don't have anything to do, it will work out of the box.

If you are adventurous, I wrote the following bash function to display the current repository's `HEAD` build status in my prompt (will only work at the repository's root for now):

```
function prompt_head_build_status {
  if [ ! -d .git ]; then
    return 0
  fi

  current_commit=$( git rev-parse HEAD )

  status=$( grep $current_commit .ciboulette.builds | awk '{print $2}' )

  case "$status" in
    fail)
      echo "/o\\"
      ;;
    pass)
      echo "\o/"
      ;;
    *)
      echo "???"
      ;;
  esac
}
```