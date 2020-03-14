# Git commit template

## Process

- create new file named .git-commit-message in the root of local project
- provide necessary permission for accessing file (sudo chmod 777 ..git-commit-message)
- copy and pest contents bellow into .git-commit-message

```
# tag: #ticket_no - message, status
# example commit message:  `dev: #123 - add refresh_token in auth module, done`
# tag can be 
#    ftr    (new feature)
#    dev  (development on feature)
#    fix    (bug fix)
#    ref     (refactoring code)
#    stl    (formatting, missing semi colons, etc; no code change)
#    tst    (adding or refactoring tests; no production code change)
#    dbg    (Changes in debugging code/frameworks; no production code change)

# ticket_no must be inter
# message will not more 70 characters
# status can be
#    done
#    wip
```
-Run this command to set customize commit template `git config --local commit.template .git-commit-message`

# Git pre-commit Hook

## Process
- create a file named `pre-commit` in `.git/hooks`
- provide necessary permission to this file (`sudo chmod 777 pre-commit`)
- copy and pest content bellow into `pre-commit` file

```
#!/usr/bin/env bash
# running test
echo ">>> Running test"
npm run test
if [ $? != 0 ]; then
  echo "Please resolve test cases first"
  exit 1
fi

# running linter
npm run lint
if [ $? != 0 ]; then
echo "Please resolve linter errors first"
    exit 1
fi
```

- create a file named `pre-commit` in `.git/commit-msg`
provide necessary permission to this file (`sudo chmod 777 commit-msg`)
copy and pest content bellow into `commit-msg` file

```
#!/usr/bin/env bash

test "" = "$(grep '^Signed-off-by: ' "$1" |
     sort | uniq -c | sed -e '/^[   ]*1[    ]/d')" || {
    echo >&2 Duplicate Signed-off-by lines.
    exit 1
}
INPUT_FILE=$1
START_LINE=`head -n1 $INPUT_FILE`
# echo "$START_LINE"
dev="^(dev): #[[:digit:]]+- "
ftr="^(ftr): #[[:digit:]]+- "
fix="^(fix): #[[:digit:]]+- "
ref="^(ref): #[[:digit:]]+- "
stl="^(stl): #[[:digit:]]+- "
tst="^(tst): #[[:digit:]]+- "
dbg="^(dbg): #[[:digit:]]+- "
if ! [[ "$START_LINE" =~ $ftr ]] && ! [[ "$START_LINE" =~ $dev ]] && ! [[ "$START_LINE" =~ $fix ]] && ! [[ "$START_LINE" =~ $ref ]] && ! [[ "$START_LINE" =~ $stl ]] && ! [[ "$START_LINE" =~ $tst ]] && ! [[ "$START_LINE" =~ $dbg ]]; then
  echo "Bad commit message, see example: dev-123: commit message"
  exit 1
fi
```