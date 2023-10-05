# antibody bin tests

## Setup

```zsh
% # Pipe test output to subenv to substitute environment vars
% subenv() { sed "s|${(P)1}|\$$1|g"; }
% export ANTIBODY_TEST_MODE=1
% [[ $OSTYPE == darwin* ]] && CACHEDIR=$HOME/Library/Caches || CACHEDIR=$HOME/.cache
% # rm -rf -- $(./bin/antibody home)
%
```

## Help

```zsh
% ./bin/antibody --help
usage: antibody [<flags>] <command> [<args> ...]

The fastest shell plugin manager

Flags:
  -h, --help           Show context-sensitive help.
  -v, --version        Show application version.

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
antibody version dev
% ./bin/antibody --version
antibody version dev
%
```

## Home

```zsh
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

```zsh
% bundles=('zsh-users/zsh-completions kind:fpath' 'zsh-users/zsh-history-substring-search kind:clone')
% print -rl -- $bundles | ./bin/antibody bundle | subenv CACHEDIR
fpath+=( $CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zsh-users-SLASH-zsh-completions )

%
```

```zsh
% zsh_plugins=$PWD/tests/testdata/antibody/bundles.txt
% ./bin/antibody bundle <$zsh_plugins | subenv CACHEDIR
TODO
%
```

## List

```zsh
% ./bin/antibody list | cut -c 66- | subenv CACHEDIR
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-dracula-SLASH-zsh
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-mattmc3-SLASH-antidote
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-mattmc3-SLASH-zman
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-ohmyzsh-SLASH-ohmyzsh
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-peterhurford-SLASH-up.zsh
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-romkatv-SLASH-zsh-bench
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-rummik-SLASH-zsh-tailf
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-rupa-SLASH-z
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-sindresorhus-SLASH-pure
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zdharma-continuum-SLASH-fast-syntax-highlighting
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zsh-users-SLASH-antigen
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zsh-users-SLASH-zsh-autosuggestions
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zsh-users-SLASH-zsh-completions
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zsh-users-SLASH-zsh-history-substring-search
$CACHEDIR/antibody/https-COLON--SLASH--SLASH-github.com-SLASH-zsh-users-SLASH-zsh-syntax-highlighting
% ./bin/antibody list | cut -c -65 | tr -d ' '
https://github.com/dracula/zsh
https://github.com/mattmc3/antidote
https://github.com/mattmc3/zman
https://github.com/ohmyzsh/ohmyzsh
https://github.com/peterhurford/up.zsh
https://github.com/romkatv/zsh-bench
https://github.com/rummik/zsh-tailf
https://github.com/rupa/z
https://github.com/sindresorhus/pure
https://github.com/zdharma-continuum/fast-syntax-highlighting
https://github.com/zsh-users/antigen
https://github.com/zsh-users/zsh-autosuggestions
https://github.com/zsh-users/zsh-completions
https://github.com/zsh-users/zsh-history-substring-search
https://github.com/zsh-users/zsh-syntax-highlighting
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
% ./bin/antibody init | subenv PWD | sed -e 's|\t|  |g'
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
% zstyle -d ':antibody:tests' set-warn-options
%
```
