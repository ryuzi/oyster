# Oyster - shell script framework library

[![Build Status](https://travis-ci.org/ryuzi/oyster.svg?branch=travis-ci)](https://travis-ci.org/ryuzi/oyster)

> Oyster provides a simple framework for using, developing, maintaining bash-based shell script for your daily work.


## Getting Started

### Prerequisites

* Unix-based operating system (OS X or Linux)
* `curl` or `wget` should be installed
* `git` should be installed

### Basic Installation

Oyster is installed by running one of the following commands in your terminal. You can install this via the command-line with either `curl` or `wget`.
Then reloading .bashrc file.

#### via curl

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ryuzi/oyster/master/install)"
```

#### via wget

```shell
sh -c "$(wget https://raw.githubusercontent.com/ryuzi/oyster/master/install -O -)"
```

## Using Oyster

### Writing a shell script

For example, create `/path/to/myproject/myscript` with these contents:

```
#!/usr/bin/env oyster
#              ^^^^^^ important!

# speak [loc]
speak() {
    local loc lang
    loc="${1:-}"

    case "${loc}" in
        en) lang="English";;
        ja) lang="Japanese";;
        *)  lang="freely";;
    esac

    echo "speak ${lang}."
}

# calc [expression]
calc() {
    local exp
    exp="$*"
    exp="${exp/x/*}"
    echo $((exp))
}
```

You can run usage by using command:

```
$ ./myscript

Usage:
    ./myscript speak                # speak [loc]
    ./myscript calc                 # calc [expression]

oyster shell script framework library:

    ENVIRONMENT: (not specified)
    APP_PATH:    /path/to/myproject
    OYST_PATH:   /your/home-dir/.oyster

[myscript@DD/MM/YYYY:hh:mm:ss] complete myscript.                  [    OK    ]

```

Your can also run by:

```
$ ./myscript speak
speak freely.
[myscript@DD/MM/YYYY:hh:mm:ss] complete myscript.                  [    OK    ]
$ ./myscript speak ja
speak Japanese.
[myscript@DD/MM/YYYY:hh:mm:ss] complete myscript.                  [    OK    ]
```

### Settings (environment variables)

If `$APP_PATH/settings/default` exists, auto load this settings first.
And also exists `$APP_PATH/settings/$ENVIRONMENT`, auto load this settings too. 

ENVIRONMENT is used depending on run environment: production, develop, linux, osx, etc.

### Modules

If modules exists in `$APP_PATH/modules/*`, it can import in myscript.

#### Testing Module

For example, create `/path/to/myproject/tests` with these contents:

```
#!/usr/bin/env oyster
import testing
source myscript

# test speak
tests.speak() {
    assert $LINENO "$(speak)" = "speak freely."
    assert $LINENO "$(speak)" = "speak fleely."
    assert $LINENO "$(speak en)" = "speak English."
    assert $LINENO "$(speak ja)" = "speak Japanese."
}

# test calc
tests.calc() {
	assert $LINENO "$(calc)" = 0
	assert $LINENO "$(calc 1 + 3)" = 4
	assert $LINENO "$(calc 2 x 3)" = 6
	assert $LINENO "$(calc 10 / 2)" = 5
}
```

You can run usage by using command:

```
$ ./tests run

tests.speak: .F..
tests.calc: ....

-----
Fail:
tests.speak(tests at line 8): test expression speak freely. = speak fleely.

-----
Ran 2 tests.

[tests@DD/MM/YYYY:hh:mm:ss] error = 1                             [ FAILURE  ]

```

#### Testing Module Installation

Testing Module is installed by running one of the following commands in your terminal. You can install this via the command-line with either `curl` or `wget`.
Then reloading .oystrc file.

#### via curl

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ryuzi/oyster-modules/master/install)"
```

#### via wget

```shell
sh -c "$(wget https://raw.githubusercontent.com/ryuzi/oyster-modules/master/install -O -)"
```

## LICENSE

Oyster is released under an MIT-style license; see `LICENSE` for details.
