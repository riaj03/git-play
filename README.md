# Git commit template

>## Process

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
- Run this command to set customize commit template `git config --local commit.template .git-commit-message`

# Git pre-commit Hook

>## Process
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

COMMIT_MSG=`grep -o '^[^(^#)|^(\*$)]*' $1`
echo "$COMMIT_MSG"
SAVEIFS=$IFS
IFS=$'\n'
LINES=($COMMIT_MSG)
IFS=$SAVEIFS

LINE_COUNT=$(expr "${#LINES[@]}" - 1)

if [ "$LINE_COUNT" -lt 3 ]; then 
  echo "Single line commit message is not allowed."
  exit 1
fi

COMMIT_TITLE=${LINES[0]}
# echo ${#COMMIT_TITLE}

if [ "${#COMMIT_TITLE}" -gt 72 ]; then 
  echo "Commit title must be less than or eqaul 72 char."
  exit 1
fi
add="^(ADD):"
dev="^(DEV):"
ftr="^(FTR):"
fix="^(FIX):"
ref="^(REF):"
stl="^(STL):"
tst="^(TST):"
dbg="^(DBG):"

if ! [[ "$COMMIT_TITLE" =~ $ftr ]] && ! [[ "$COMMIT_TITLE" =~ $dev ]] && ! [[ "$COMMIT_TITLE" =~ $fix ]] && ! [[ "$COMMIT_TITLE" =~ $ref ]] && ! [[ "$COMMIT_TITLE" =~ $stl ]] && ! [[ "$COMMIT_TITLE" =~ $tst ]] && ! [[ "$COMMIT_TITLE" =~ $dbg ]] && ! [[ "$COMMIT_TITLE" =~ $add ]]; then
  echo "Bad commit message, here is good example message: 

    DEV: Waris User model
    Functional description:
    - added new model file users.ts for writing the user related business logics
    - imported user.ts in user.controller.ts to consume the user functions
  "
  exit 1
fi
```
