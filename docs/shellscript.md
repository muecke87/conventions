# Shellscript (BASH)

## Table of Contents

1. [Shellcheck](#shellcheck)
1. [Avoid mistakes by using the proper syntax](#avoid-mistakes-by-using-the–proper-syntax)
1. [Code readability](#code-readability)
1. [File layout](#file-layout)

## Shellcheck

* Use [shellcheck](https://github.com/koalaman/shellcheck) to find errors in your scripts.
* As a general rule: shellcheck is correct - you are wrong - fix the code ;-)
    * There will be some exceptions. [How to disable a certain check for a line](https://github.com/koalaman/shellcheck/wiki/Ignore).
* Helpful descriptions for each error code are found in the wiki. E.g. [SC2068](https://github.com/koalaman/shellcheck/wiki/SC2068)

## Avoid mistakes by using the proper syntax

1. verify your scripts with [Shellcheck](#shellcheck). 
1. the following subchapters describe how to avoid the most common problems.
1. if there are conflicts between the rules below and shellcheck, shellcheck wins. 

### Declare variables with `local` or `readonly`

Variables intended as constants should be declared with `readonly` and uppercase:

    readonly DAYS_IN_WEEK=7

Variables defined inside functions should be declared with `local` to restrict scope to current function.

```bash
foo() {
    local bar=100
    
    echo "hello: $bar"
}

foo 
# prints: hello 100
echo "hello: $bar" 
# prints: hello
```

You can use both together with `local -r`

```bash
foo() {
    local -r BAR=100

    # the following statement will fail:
    BAR=200

    echo "hello: $BAR"
}

foo 
# prints: hello: 100

echo "hello outside: $BAR" 
# prints: hello outside:
```

### Using variables: with apostrophes and brackets

Quote variable in apostrophes and wrapp in brackets:

```bash
echo "${foo}"
cp "${foo}" "${bar}"
```

* `{}` clearly separates variable names from other characters
* `""` prevents WordSplitting and globbing

Exception:

```bash
# for [[ .. ]] its okay to not use apostrophes, since WordSplitting is disabled.
[[ -f $file ]] && echo "$file is a file"
```

### if statements: prefer `[[` over `[`

`[[` is less error prone. The subtle differences are explained here: http://mywiki.wooledge.org/BashFAQ/031

```bash
# bad
[ -f "$file" ] && echo "$file is a file"
 
 # good
[[ -f $file ]] && echo "$file is a file"
```

### Use `"${*}"` instead of `"${@}"`

Altough they are very similar `"${*}"` is to prefer since it gives a string and not an array.

Example:

```bash
demo() {
    local all_arguments=${*}
    
    echo "${all_arguments}"
}

demo 1 2 3 4 5
# prints: 1 2 3 4 5
```

## Code readability

## Use long option-/parameter-names


Improve readability of options/parameters by using long variant:

```bash
# bad
rm --force
ln -fs

# good
rm -f
ln --force --symbolic
```

(order options by alphabet if possible)

## File layout

## Provide file header

Example:

```bash
########################################################################
# Sends a mail using mailx (heirloom-mailx)
# Seperate multiple mail accounts with a space
#
# Arguments:
#     - recepient: emails (separate multiple addresses with a space)
#     - subject: email subject
#     - body: email body text
#
# Usage:
#     send_mail "recepient@gmail.com" "subject" "body"
########################################################################
```
 
Usage: if you have a usage method, you can refer to that method 

```bash
# Usage:
#     see method "usage" or execute script without parameters
########################################################################
```




