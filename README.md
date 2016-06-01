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
$ chmod u+x myscript
$ ./myscript help

Usage:
    ./myscript speak                # speak [loc]
    ./myscript calc                 # calc [expression]

oyster shell script framework library:

    ENVIRONMENT:      (not specified)
    APP_PATH:         /path/to/myproject
    OYST_PATH:        /your/home-directory/.oyster
```

You can also run by:

```
$ ./myscript speak
speak freely.
$ ./myscript speak ja
speak Japanese.
```

### Oyster Settings file

When you use Oyster, you have to tell it which settings you're using.
Do this by using an environment variable, ENVIRONMENT.

```
$ cd /path/to/myproject
$ mkdir settings
$ echo "favorite_sport=baseball" > settings/default
```

and modify `/path/to/myproject/myscript` with these contents:

```
#!/usr/bin/env oyster

# speak [loc]
speak() {
	...
}

# calc [expression]
calc() {
	...
}

# echo favorite sports
sport() {
	echo "My favorite sport is ${favorite_sport:-}."
}
```

You can run by:

```
$ ./myscript sport
My favorite sport is baseball.
```

If `/path/to/myproject/settings/default` exists, auto load this settings first.
And also exists `/path/to/myproject/settings/$ENVIRONMENT`, auto load this settings too.

for example,

```
$ echo "favorite_sport=football" > settings/highschool
$ echo "favorite_sport=basketball" > settings/university
```

then, you can run by:

```
$ export ENVIRONMENT=highschool
$ ./myscript sport
My favorite sport is football.
$ export ENVIRONMENT=university
$ ./myscript sport
My favorite sport is basketball.
```

ENVIRONMENT may use depending on run environment: production, develop, linux, osx, etc.

### Oyster Modules

To use module, please install the [oyster-modules](https://github.com/ryuzi/oyster-modules).



### Custom Module

Your created module is stored in `/path/to/myproject/modules`.
For example, create `/path/to/myproject/modules/checksum` with these contents:

```
#!/usr/bin/env bash
require openssl || die "openssl is require."

sha1() {
    local filename
    [ -z "${1:-}" ] && return 1
    filename="$1"
    openssl sha1 "$filename" | cut -d ' ' -f 2
}
```

and modify `/path/to/myproject/myscript` with these contents:

```
#!/usr/bin/env oyster

# speak [loc]
speak() {
	...
}

# calc [expression]
calc() {
	...
}

# echo favorite sports
sport() {
	...
}

# echo checksum
mysha1() {
    import checksum || die "import checksum error"
    local result
    result=$(sha1 "myscript" || die "filename is not specified.")
    echo "$result"
}
```

then, you can run by:

```
$ ./myscript mysha1
ef95cf43ecdb62b6ef0a5d9145089ed3d1dd0829
```


## LICENSE

Oyster is released under an MIT-style license; see `LICENSE` for details.
