KANBAN.bash 
===========
commandline asciii kanban board for minimalist productivity bash hackers (csv-based)

## Install

    $ wget "https://github.com/coderofsalvation/kanban.bash/blob/master/kanban"
    $ chmod 755 kanban
  
## Show me the kanban board!

    $ ./kanban add TODO PERSONAL buy rose for girlfriend foo bar
    $ ./kanban show

<img alt="" src=".res/board.png"/>

> NOTE: columns are configurable, and board resizes according to terminal width

## Change status 

    $ ./kanban show
    $ ./kanban 34 DONE
    IN_PROGRESS -> DONE

## Edit item 

    $ ./kanban 34

> NOTE: make sure you have your favorite editor set in ~/.bashrc : 'export EDITOR=vim' etc

## Kanban board grep 

    $ ./kanban show PERSONAL | grep -v deadline

This will show a kanban board with todo's which only match 'PERSONAL' excluding deadlines.
Nice to get project-specific kanban overviews.

## Simple listing of status 

> NOTE: from here we use the k-alias, see the 'Attention Unix ninjas' on how to use it 

    $ k TODO 
    TODO  ISD   Lorem                       senectus
    TODO  PJGE  Lorem ipsum dolor sit amet  consectetuer adipiscing                   
    TODO  ISD   Lorem ipsum dolor sit amet  Phasellus
    TODO  NINJW workout                     2024-04-08 deadline

    $ k BACKLOG | grep ISD | grep -v deadline
    TODO  ISD   Lorem ipsum dolor sit amet  Phasellus
    TODO  ISD   ipsum dolor sit amet  Phasellus
    TODO  ISD   dolor sit amet  Phasellus

## Configuration 

see ~/.kanban.conf (gets created automatically).
You can define the kanban statuses, and limit the maximum amount of todos per status.

## Usage

    $ kanban
    Usage:

      kanban show [grep string]               # show ascii kanban board
      kanban <id>                             # edit or update item 
      kanban <id> <status>                    # update status of todo id (uses $EDITOR as preferred editor)
      kanban <status>                         # show only todo items with this status 
      kanban add <status> <tag> <info> ...    # add item 

      NOTE #1: statuses can be managed in ~/.kanban.conf
      NOTE #2: the database csv can be found in ~/.kanban.csv

    Environment:

      You can switch context (e.g. work vs home vs project x ) like so:

      KANBANFILE=~/.kanban.foo.csv kanban show

## Attention UNIX ninjas 

> type 'k' instead of './kanban' 

    $ cp kanban ~/bin 
    $ echo 'export PATH=$PATH:~/bin' >> ~/.bashrc
    $ echo 'alias k=kanban'          >> ~/.bashrc
    $ source ~/.bashrc

(now all terminals will recognize 'k' as a command)

> Cleanup your kanban board with some bash-fu:

    $ for i in {19,36,49}; do kanban $i BACKLOG; done
    DONE -> BACKLOG
    DONE -> BACKLOG
    DONE -> BACKLOG

> mass-renames:

    $ sed -i 's/FOO/BAR/g' ~/.kanban.csv
    
> Open a terminal on an extra monitor/screen/tmux:

    $ watch kanban show

> Run ninja-commands like: 'k 23 DONE' and withness the update:

    $ k 34 DONE 
    TODO -> DONE
    $ k add TODO NINJW workout" "$(date --date='tomorrow' +'%Y-%m-%d') deadline"
    
## Why 

> *For developers, there's no such thing as the ultimate todo-utility*

KANBAN.bash brings the lean and mean kanban board to the console.
It uses csv as database backend, a very popular tabular format.
The commandline usage is very minimal so few keystrokes can do magic.

## Todo 

* more testing
* tab completion
* easier way of adding todos
