# VIM
## Navigate in Vim
- h- left  
- j- down  
- l- right  
- k- up  

`ESC :q!` ->exits the editor, discarding any changes  
`ESC :wq` ->exits the editor,saving any changes  

press `x` to delete the character under the cursor  
press `i` to insert text (esc te return to Normal mode)  
press `a` to append text (esc te return to Normal mode)  
press `dw` to delete a word  
press `d$` to delete to the end of the line  

typing a number before a motion repeats it that many times  
example: 
- 2w -> to move the cursor two words forward  
- 0 -> to move to the start of the line  

### d - delete operator  
type `dd` to delete a whole line  
type `2dd` to delete 2 lines  
type `u` to undo the last commands - `U` to fix a whole line  
`CTRL-R` to redo the commands(undo the undos)  

### c - change operator  
type `p` to put previously deleted text after the cursor  
type `rx` to replace the character at the cursor with  x  
type `ce` to change until the end of a word  
`cc`  does the same for the whole line  

Type `CTRL-G` to show your location in the file and the file status  
Press  `G`  to move you to the bottom of the file.  
Type  `gg`  to move you to the start of the file  
Type the number of the line you were on and then  G .  This will return you to the line you were on when you first pressed CTRL-G  

Type  `/`  followed by a phrase to search for the phrase  
To search for the same phrase again, simply type  `n`  
To search for the same phrase in the opposite direction, type  `N`  
To search for a phrase in the backward direction, use  `?`  instead of  `/`  
To go back to where you came from press  `CTRL-O`  .Repeat to go back further.  `CTRL-I` goes forward  

Place the cursor on any (, [, or {  ->  type the  `%`  character -> The cursor will move to the matching parenthesis or bracket  

To substitute new for the first old in a line type
 ` :s/old/new  `  
To substitute new for all 'old's on a line type   `:s/old/new/g`  
To substitute phrases between two line #'s type `:#,#s/old/new/g`  
To substitute all occurrences in the file type `:%s/old/new/g`  
To ask for confirmation each time add 'c'    `:%s/old/new/gc`  

Type  :!  followed by an external command to execute that command.  
To save the changes made to the text, type  :w FILENAME  
Now remove the file by typing (Windows):   `:!del TEST` or (Unix):      `:!rm TEST`  
To save part of the file, type  `v` motion  `:w FILENAME`  
`:r FILENAME`  retrieves disk file FILENAME and puts it below the cursor position.  
`:r !dir`  reads the output of the dir command and puts it below the cursor position.  

Type  `o`  to open a line below the cursor and place you in Insert mode  
To open up a line ABOVE the cursor, simply type a capital  `O`  

Type  `a`  to insert text AFTER the cursor.  
Type  A  to insert text after the end of the line.  

The  e  command moves to the end of a word.  

a, i and A all go to the same Insert mode, the only difference is where the characters are inserted  

Type a capital  R  to replace more than one character. Replace mode is like Insert mode, but every typed character deletes an existing character  

Start Visual mode with  v  and move the cursor  
Type  y  to yank (copy) the highlighted text.  
Type  p  to put (paste) the text  
You can also use  y  as an operator:  yw  yanks one word, yy  yanks the whole line, then  p  puts that line  

Typing ":set xxx" sets the option "xxx".  Some options are:  
    'ic' 'ignorecase'       ignore upper/lower case when searching  
    'is' 'incsearch'        show partial matches for a search phrase  
    'hls' 'hlsearch'        highlight all matching phrases  

Prepend "no" to switch an option off:   :set noic  
