# Spacemacs

## Useful Spacemacs commands

* `SPC q q` - quit
* `SPC w /` - split window vertically
* `SPC w` - - split window horizontally
* `SPC 1`   - switch to window 1
* `SPC 2`   - switch to window 2
* `SPC w c` - delete current window
* `SPC TAB` - switch to previous buffer
* `SPC b b` - switch buffers
* `SPC f f` - find a file
* `SPC f s` - save a file \(:w also works\)
* `SPC p p` - open project
* `SPC p h` - find a file in current project
* `SPC b d` - kill current buffer
* `SPC b M` - move buffer to another window
* `SPC v`   - enter expand-region mode
* `SPC b b` - Helm mini; lists buffers & recent files
  * `CTRL SPC` - Mark Items
  * `CTRL z` - Actions
* `SPC b B` - ibuffer
* `SPC f f` - open files
  * `CTRL h` - up a folder
  * `CTRL l` - open a folder
  * `CTRL j` - up
  * `CTRL k` - down
* `SPC p f` - opens root of project
* `SPC p p` - opens projects
* `SPC /` - searches through project
* `SPC s s` - search in a file
* `SPC s l` - find all function definitons in a file
* `SPC v` - expand region
* `SPC V` - contract region
* `s (` - put parens around a region
* `SPC s e` - multiple cursors
  * `n` - jump
  * `N` - jump
* `SPC h d` - help describe
* `SPC h d f` - help describe functions
* `SPC h d v` - help describe variables
* `SPC f e h` - help
* `ALT /` - snippet completion
* `SPC t s` - syntax checking
* `SPC e` - syntax checking options
* `SPC a r` - ranger
* `SPC a d` - deer

## unimpaired

* `[e` - Move line up
* `]e` - Move line down
* `[SPACE` - Insert space above
* `]SPACE` - Insert space below
* `[p` - Paste above current line
* `]p` - Paste below current line

## evil-mc

* `grm` - make-all-cursors
* `gru` - undo-all-cursors
* `grs` - pause-cursors
* `grr` - resume-cursors
* `grf` - make-and-goto-first-cursor
* `grl` - make-and-goto-last-cursor
* `grh` - make-cursor-here
* `M-n` - make-and-goto-next-cursor
* `grN` - skip-and-goto-next-cursor
* `M-p` - make-and-goto-prev-cursor
* `grP` - skip-and-goto-prev-cursor
* `C-n` - make-and-goto-next-match
* `grn` - skip-and-goto-next-match
* `C-t` - skip-and-goto-next-match
* `C-p` - make-and-goto-prev-match
* `grp` - skip-and-goto-prev-match

## Eyebrowse

* `gt` - go to next workspace
* `gT` - go to previous workspace
* `SPC l w n` - create or switch to workspace n
* `SPC l w TAB` - switch to last active workspace
* `SPC l w c` - close current workspace
* `SPC l w n` or `SPC l w l` - switch to next workspace
* `SPC l w N` or `SPC l w p` or `SPC l w h` - switch to previous workspace
* `SPC l w r` - set a tag to the current workspace
* `SPC l w w` - switched to tagged workspace

## Find/Replace

* `Alt %`    - query-replace; active region, or cursor point to end    interactive find/replace
  * `y` - do the replacement.
  * `n` - skip
  * `!` - do this and all remaining replacements without asking.
  * `Ctrl+g` - cancel. 

## Git

SPC g b    open a magit blame  
SPC g B    quit magit blame  
SPC g c    commit changes  
SPC g C    checkout branches  
SPC g d    show diff prompt  
SPC g D    show diff against current head  
SPC g e    show ediff comparison  
SPC g E    show ediff against current head  
SPC g f    show fetch prompt  
SPC g F    show pull prompt  
SPC g H c    clear highlights  
SPC g H h    highlight regions by age of commits  
SPC g H t    highlight regions by last updated time  
SPC g i    git init a given directory  
SPC g I    open helm-gitignore  
SPC g l    open a magit log  
SPC g L    display the log for a file  
SPC g P    show push prompt  
SPC g s    open a magit status window  
SPC g S    stage current file  
SPC g m    display the last commit message of the current line  
SPC g t    launch the git time machine  
SPC g U    unstage current file  
Highlight by age of commit or last update time is provided by smeargle.  
Git time machine is provided by git-timemachine.  
Git last commit message per line is provided by git-messenger.  
3.1 Magit

Spacemacs uses magit to manage Git repositories.

To open a status buffer, type in a buffer of a Git repository: SPC g s

Spacemacs uses evil-magit for key bindings in magit buffers \(unless your editing style is set to emacs, in which case you get the default magit bindings\), which are the standard magit key bindings with some minimal changes to make them comfortable for evil users.

Here are the often used bindings inside a status buffer:

Key Binding    Description  
/    evil-search  
$    open command output buffer  
c c    open a commit message buffer  
b b    checkout a branch  
b c    create a branch  
f f    fetch changes  
F \(r\) u    pull tracked branch and rebase  
gr    refresh  
j    goto next magit section  
C-j    next visual line  
k    goto previous magit section  
C-k    previous visual line  
l l    open log buffer  
n    next search occurrence  
N    previous search occurrence  
o    revert item at point  
P u    push to tracked branch  
P m    push to matching branch \(e.g., upstream/develop to origin/develop\)  
q    quit  
s    on a file or hunk in a diff: stage the file or hunk  
x    discard changes

* on a hunk: increase hunk size
* on a hunk: decrease hunk size
  S    stage all
  TAB    on a file: expand/collapse diff
  u    on a staged file: unstage
  U    unstage all staged files
  v or V    select multiple lines
  z z    stash changes
  3.2 Staging lines

Magit allows you to stage specific lines by selecting them in a diff and hitting s to stage. Due to inconsistencies between Vim and Emacs editing styles, if you enter visual line state with V, you will stage one more line than intended. To work around this, you can use v instead \(since Magit only stages whole lines, in any case\).

3.3 Commit message editing buffer

In a commit message buffer press ~,c~ \(if dotspacemacs-major-mode-leader-key is ~,~\) or C-c C-c to commit the changes with the entered message. Pressing ~,a~ or C-c C-k will discard the commit message.

Key Binding    Description  
h    go left  
j    go down  
k    go up  
l    go right  
3.4 Interactive rebase buffer

Key Binding    Description  
c or p    pick  
e    edit  
f    fixup  
j    go down  
gj    move line down  
k    go up  
gk    move line up  
d or x    kill line  
r    reword  
s    squash  
u    undo  
y    insert  
!    execute  
3.5 Quick guide for recurring use cases in Magit

Amend a commit:  
l l to open log buffer  
c a on the commit you want to amend  
~,c~ or C-c C-c to submit the changes  
Squash last commit:  
l l to open log buffer  
r e on the second to last commit, it opens the rebase buffer  
j to put point on last commit  
s to squash it  
~,c~ or C-c C-c to continue to the commit message buffer  
~,c~ or C-c C-c again when you have finished to edit the commit message  
Force push a squashed commit:  
in the status buffer you should see the new commit unpushed and the old commit unpulled  
P -f P for force a push \(beware usually it is not recommended to rewrite the history of a public repository, but if you are sure that you are the only one to work on a repository it is ok - i.e. in your fork\).  
Add upstream remote \(the parent repository you have forked\):  
M to open the remote popup  
a to add a remote, type the name \(i.e. upstream\) and the URL  
Pull changes from upstream \(the parent repository you have forked\) and push:  
F -r C-u F and choose upstream or the name you gave to it  
P P to push the commit to origin  
3.6 Git-Flow

magit-gitflow provides git-flow commands in its own magit menu.

Key Binding    Description  
%    open magit-gitflow menu  
3.7 Git time machine

git-timemachine allows to quickly browse the commits of the current buffer.

Key Binding    Description  
SPC g t    start git timemachine and initiate micro-state  
c    show current commit  
n    show next commit  
N    show previous commit  
p    show previous commit  
q    leave micro-state and git timemachine  
Y    copy current commit hash

## Useful Vim key bindings

### movement

* `0` - beginning of line
* `^` - beginning of non-whitespace
* `$` - end of line
* `9j` - move down 9 lines
* `w` - move forward by word
* `b` - move backward by word
* `gg` - first line
* `G` - last line
* `C-u` - up half page
* `C-d` - down half page
* `f/` - move forward to first "/" character
* `t/` - move forward right before the first "/" character
* `;` - repeat that command again
* `H` - head of the screen
* `M` - middle of the screen
* `L` - last of the screen
* `}` - move forward by paragraph or block
* `{` - move backwards by paragraph or block
* `*` - search for word under the cursor
* `n` - search again forward
* `N` - search again backwards
* `#` - search backwards for word under cursor
* `/` - search forward
* `?` - search backward
* `%` - find matching brace, paren, etc
* `ma` - mark a line in a file with marker "a"
* ```a`` - after moving around, go back to the exact position of marker "a"
* `'a` - after moving around, go back to line of marker "a"
* `:marks` - view all the marks
* `''` - go to the last place you were

### editing

* `x` - delete char under cursor
* `X` - delete char before cursor
* `A` - add to end of line
* `I` - insert at the beginning of the line
* `dd` - delete line
* `D` - delete from cursor to end of line
* `di'` - delete text inside single quotes
* `yy` - copy line
* `Y` - copy from cursor to end of line
* `cc` - change line
* `C` - change from cursor to end of line
* `cit` - change text inside html tag
* `ci'` - change text inside single quotes
* `ci{` - change text inside curly brackets.
* `ci...` - etc
* `p` - paste after cursor
* `P` - paste before cursor
* `o` - add line below
* `O` - add line above
* `.` = repeat last comment
* `r` - replace character
* `R` - replace. \(overwrite\) \(good for columns of text\)
* `J` - join line \(cursor can be anywhere on line\)

### visual mode

* `v` - visual char mode
* `V` - visual line mode
* `C-v` - block visual mode



