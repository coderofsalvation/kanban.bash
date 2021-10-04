<img alt="" src="https://api.travis-ci.org/coderofsalvation/kanban.bash.svg"/>
<br>
<img alt="" src=".res/carbon.png"/>

commandline kanban #notetaking #todomanager #scriptable for teams AND personal CSV kanban for minimalist productivity hackers.

> **WHY?** online issuetrackers are great for teams, but how to manage (personal) todo's on a crossrepo-, macro-, or micro-level? This is a very simple **powerful** tool to do that **AND** measure productivity. Just store the 
CSV-file(s) inside repos, clouddrives and local filesystems. #teamfriendly #symlinktheplanet #nestedkanbans

## Install

```bash
$ curl -LO "https://raw.githubusercontent.com/coderofsalvation/kanban.bash/master/kanban"
$ chmod 755 kanban
```
  
## Show me the kanban board!

```bash
$ ./kanban add TODO PERSONAL "buy rose for girlfriend foo bar"
$ ./kanban show
```

<img alt="" src=".res/board.gif"/>

> NOTE: columns are configurable, and board resizes according to terminal width

## Usage 

```bash
$ ./kanban
Usage:

  kanban init                             # initialize kanban in current directory
  kanban add                              # add item interactive (adviced) 
  kanban show [status] ....               # show ascii kanban board [with status]
  kanban <id>                             # edit or update item 
  kanban <id> <status>                    # update status of todo id (uses $EDITOR as preferred editor)
  kanban <status> .....                   # list only todo items with this status(es)
  kanban list                             # list all todos (heavy)
  kanban tags                             # list all submitted tags
  kanban add <status> <tag> <description> # add item (use quoted strings for args)  
  kanban stats status [tag]
  kanban stats tag 
  kanban stats history 
  kanban csv                              # edit raw csv

  NOTE #1: statuses can be managed in ~/.kanban/.kanban.conf
  NOTE #2: the database csv can be found in ~/.kanban/.kanban.csv

Examples:

  kanban add TODO projectX "do foo"
  kanban TODO DOING HOLD                 
  kanban stats status projectX
  kanban stats tag projectX 
  watch NOCOLOR=1 kanban show
  # notekeeping by entering a filename as description:
  echo hello > note.txt && kanban add DOING note.txt
  # store in github repo
  git clone https://../foo.git && cd foo.git && kanban init && git add .kanban

Environment:

  X=120 kanban ....         # set max line-width to 120
  NOCOLOR=1 kanban ....     # disable colors
  PLAIN=1 kanban ...        # plaintext, disable utf8 chars
```

## Change status 

```bash
$ ./kanban show
.------.              .------.              .-------.
| TODO |_______       | HOLD |_______       | DOING |_______
|                     |                     |
| 1 foobar                                  | 3 ipsum
| 2 lorem                                   

$ ./kanban 2 DOING
TODO -> DOING
```

## Edit item 

> NOTE: make sure you have your favorite editor set in ~/.bashrc (`export EDITOR=vim` e.g.)

```bash
$ ./kanban 2        # this executes ${EDITOR} 
```

## Todo grep 

```bash
$ ./kanban TODO DOING | grep projectfoo 
```

Nice to get project-specific kanban overviews.

## Note-taking

> adding a filename as a description, will trigger kanban to launch `$EDITOR`:

```bash
$ echo -e "hello\nworld" > my_idea.txt
$ kanban add TODO note my_idea.txt
$ kanban list
  id  status   tag        description
  -   -        -          -                                                                                                                                                                -
  4   TODO     note       my_idea.txt
$ k 4
```

> TIP: use symlinks to share notes across boards (`cd myproject && ln -s ~/.kanban/timelog.txt timelog.txt` e.g.)

## Simple listing of status 

> NOTE: from here we use the k-alias, see the 'Attention Unix ninjas' on how to use it 

```bash
$ k TODO
id   status  tag   description                 history
-    -       -     -                           -
185  TODO    bly   fooo bar flop               BTBDHDHDHT
199  TODO    bly   meeting about techdesign    BT
245  TODO    lb    checkout testsuite          BT
246  TODO    nus   add field to db             BT
242  TODO    nus   fix db lag                  BT 
```

as you can see in the history, todo 185 is quite problematic.
It went from Backlog->Todo->Backlog->Doing->Hold->... and so on.
Obviously the person who assigned this todo should rethink it, and chop it up into seperate todos.

```bash    
$ k TODO 2015-08
id   status  tag   description                 history
-    -       -     -                           -
246  TODO    nus   add field to db             BT
242  TODO    nus   fix db lag                  BT
```

Here you can see all todo's which were 'touched' in august 2015

## Configuration 

see `~/.kanban/.kanban.conf` (gets created automatically).
You can define the kanban statuses, and limit the maximum amount of todos per status.

## Idiotproof csv-editing 

Safest way to keep the CSV sane:

```bash
$ ./kanban add
enter description:
> do laundry
enter one of statuses: BACKLOG TODO IN_PROGRESS HOLD DONE
> TODO
enter one of tags: projectA, projectB 
> projectA
$
```

## Responsive kanban.

As mentioned earlier, the status/categorynames can be changed in `.kanban/.kanban.conf`.
No widescreen? Show a tag-less, simplified kanban board by hiding some categories:

```bash
XSMALL=119                           # show simplified kanban for terminalwidth < 119 chars
SMALLSCREEN=('DOING' 'TODO' 'HOLD')  # define simplified kanban board statuses
```

## Nested kanbans

<img src="data:image/jpeg;base64,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" width="40%"/>

```bash
$ kanban init
$ mkdir featureX           
$ kanban add TODO featureX 
$ cd featureX
$ kanban init
$ kanban add TODO foobar
$ cd ..
$ kanban show
 .____.
| TODO |_____
|
| 12 featureX

$ kanban 12                         # shows kanban inside featureX 
 .____.
| TODO |_____
|
| 1  foobar 
```

## Scriptable / Kanban Bot

> kanban items are **SCRIPTABLE** using your favorite language. This allows dynamic statuses, tags & descriptions in the CSV-file:

```bash
"$(~/.kanban/bot status_day TODO '1 4')","script","database backup","T","2021-10-04@15:36"
```
this will call the following (executable)  shellscript (`~/.kanban/bot`)

```bash
#!/bin/bash
status_day(){
  [[ "$2" =~ $(date +%u) ]] && printf $1 || printf BACKLOG
}

"$@"
```
Profit! <br>The `database backup` item will have status **TODO** on mondays & fridays, otherwise **BACKLOG**<br>

> TIP: use curl to generate dynamic descriptions, for example:

```bash 
"$(curl https://api.github.com/repos/coderofsalvation/kanban.bash/issues | grep total_count | sed 's/[^0-9]//g') open issues"
```

## Blinking text

Just wrap a word with stars (`*iamblinking*` e.g.) in your csv to catch more attention.

## Attention UNIX ninjas 

> type 'k' instead of './kanban' 

```bash
$ cp kanban ~/bin 
$ echo 'export PATH=$PATH:~/bin' >> ~/.bashrc
$ echo 'alias k=kanban'          >> ~/.bashrc
$ source ~/.bashrc
```

(now all terminals will recognize 'k' as a command)

> Cleanup your kanban board with some bash-fu:

```bash
$ for i in {19,36,49}; do kanban $i BACKLOG; done
DONE -> BACKLOG
DONE -> BACKLOG
DONE -> BACKLOG
```

> mass-renames:

```bash
$ sed -i 's/FOO/BAR/g' ~/.kanban.csv
```
    
> Open a terminal on an extra monitor/screen/tmux:

```bash
$ NOCOLOR=1 watch kanban show
```

> Run ninja-commands like: 'k 23 DONE' and withness the update:

```bash
$ k 34 DONE 
TODO -> DONE
$ k add TODO NINJW workout" "$(date --date='tomorrow' +'%Y-%m-%d') deadline"
```

## Statistics

With the power of grep you can get overviews:

```bash
$ k stats status

            DONE   155 ▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆ 
         BACKLOG    73 ▆▆▆▆▆▆▆▆▆▆ 
            HOLD     9 ▆▆ 
            TODO     5 ▆ 
           DOING     5 ▆ 

$ k status 2015-08

            DONE   155 ▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆ 
         BACKLOG    73 ▆▆▆▆▆▆▆▆▆▆ 
            HOLD     9 ▆▆ 
            TODO     5 ▆ 
           DOING     5 ▆ 

$ k stats status DONE 2015-08 

      projectfoo    62 ▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆ 
      opensource    43 ▆▆▆▆▆▆▆▆▆▆▆▆▆▆ 
        projectX     3 ▆ 
           admin     2 ▆ 

$ k stats status projectfoo 

            DONE    56 ▆▆▆▆▆▆▆▆ 
         BACKLOG    33 ▆▆▆▆▆ 
            HOLD     6 ▆ 
            TODO     2 ▆ 
           DOING     1 ▆ 
```

Lets see what the slacking / project ratio is :)

```bash
$ k stats tag 2015-08

         slacking   76 ▆▆▆▆▆▆▆▆ 
         projecfoo  36 ▆▆▆▆ 
```

What are are typical tasktransitions:

```bash
$ k stats history
              T   129 ▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆ 
        BTDHDHD    16 ▆▆▆ 
              T   129 ▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆ 
             BD    16 ▆▆▆  ```


View which projects were put on hold at least 2 times in 2014:

```bash
$ k stats history HDHD 2014 

   project30     6 ▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆▆ 
   project40     4 ▆▆▆▆▆▆▆▆▆▆▆▆▆▆ 
   project20     4 ▆▆▆▆▆▆▆▆▆▆▆▆▆▆ 
   project10     3 ▆▆▆▆▆▆▆▆▆▆ 
```

## Realtime Web-based kanban boards 

Create the following `index.html`:

```html
<!DOCTYPE html>
<pre id="log"></pre>
<script>
  // helper function: log message to screen
  function log(msg) { document.getElementById('log').textContent += msg + '\n'; }
  // setup websocket with callbacks
  var ws = new WebSocket('ws://localhost:8080/');
  ws.onopen = function() { console.log('CONNECT'); };
  ws.onclose = function() { console.log('DISCONNECT'); };
  ws.onmessage = function(event) { log(event.data); };
</script>
```

And then serve it to the web:

```bash
$ sudo apt-get install websocketd
$ X=120 NOCOLOR=1 PLAIN=1 websocketd -passenv 'X,NOCOLOR,PLAIN' -port 8080 -staticdir . ./kanban show
Mon, 04 Oct 2021 18:23:08 +0200 | INFO   | server     |  | Serving using application   : ./kanban show
Mon, 04 Oct 2021 18:23:08 +0200 | INFO   | server     |  | Serving static content from : .
Mon, 04 Oct 2021 18:23:08 +0200 | INFO   | server     |  | Starting WebSocket server   : ws://localhost:8080/
```

> Now surf to http://localhost:8080 and PROFIT!

## Tab completion 

Somehow source `kanban.completion` in your `~/.bashrc` or just copy it to `/etc/bash_completion.d`

    
## Why 

> *For developers, there's no such thing as the ultimate todo-utility*

KANBAN.bash brings the lean and mean kanban board to the console.
It uses csv as database backend, a very popular tabular format.
The commandline usage is very minimal so few keystrokes can do magic.

## Developer info 

tests oneliners:

* run: `cd test; for test in test-*; do ./$test &>/dev/null; done && echo OK || echo ERROR` 
* debug: `cd test; for test in test-*; do bash -x $test; done && echo OK || echo ERROR`

## Todo 

* more testing
* easier way of adding todos
