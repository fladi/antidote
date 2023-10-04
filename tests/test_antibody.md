# antibody bin tests

## Setup

```zsh
% # Pipe test output to subenv to substitute environment vars
% subenv() { sed "s|${(P)1}|\$$1|g"; }
%
```

## Help

```zsh
% ./bin/antibody --help
usage: antibody [<flags>] <command> [<args> ...]

The fastest shell plugin manager

Flags:
  -h, --help            Show context-sensitive help (also try --help-long and --help-man).
  -v, --version         Show application version.

Commands:
  help [<command>...]
    Show help.

  bundle [<bundles>...]
    downloads a bundle and prints its source line

  update
    updates all previously bundled bundles

  home
    prints where antibody is cloning the bundles

  purge <bundle>
    purges a bundle from your computer

  list
    lists all currently installed bundles

  path <bundle>
    prints the path of a currently cloned bundle

  init
    initializes the shell so Antibody can work as expected
% # short -h flag also supported
% ./bin/antibody -h #=> --lines 32
% # running the bare command shows help too
% ./bin/antibody #=> --lines 32
%
```

## Version

```zsh
% ./bin/antibody -v
antibody version 1.9.2
% ./bin/antibody --version
antibody version 1.9.2
%
```

## Home

```zsh
% [[ $OSTYPE == darwin* ]] && CACHEDIR=$HOME/Library/Caches || CACHEDIR=$HOME/.cache
% ./bin/antibody home | subenv CACHEDIR
$CACHEDIR/antibody
%
```

## Bundle

```zsh
% ./bin/antibody bundle zsh-users/zsh-autosuggestions | subenv CACHEDIR
source $CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zsh-users-SLASH-zsh-autosuggestions/zsh-autosuggestions.plugin.zsh
fpath+=( $CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zsh-users-SLASH-zsh-autosuggestions )
%
```

## Path

```zsh
% ./bin/antibody path zsh-users/zsh-autosuggestions | subenv CACHEDIR
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zsh-users-SLASH-zsh-autosuggestions
%
```

## Init

```zsh
% ./bin/antibody init | subenv PWD
#!/usr/bin/env zsh
antibody() {
  case "$1" in
  bundle)
    source <( $PWD/bin/antibody $@ ) || $PWD/bin/antibody $@
    ;;
  *)
    $PWD/bin/antibody $@
    ;;
  esac
}

_antibody() {
  IFS=' ' read -A reply <<< "help bundle update home purge list init"
}
compctl -K _antibody antibody
%
```

## Teardown

```zsh
% unfunction subenv
%
```
