**commands**
  command  | description
  ---------|-------------
ls -a 	| list all files including hidden file starting with '.'
ls --color| 	colored list [=always/never/auto]
ls -d |	list directories - with ' */'
ls -F |	add one char of */=>@| to enteries
ls -i |	list file's inode index number
ls -l |	list with long format - show permissions
ls -la| 	list long format including hidden files
ls -lh| 	list long format with readable file size
ls -ls| 	list with long format with file size
ls -r |	list in reverse order
ls -R |	list recursively directory tree
ls -s |	list file size
ls -S |	sort by file size
ls -t |	sort by time & date
ls -X |	sort by extension name
file filename | determine the type of a file
file -s specialfilename | used for special files such as /dev/sda, ...
file -z compressedfile | used for compressed files
less filename | read contents of text file one page(one screen) per time
less -N filename | shows linenumbers
**less navigationkeys** | navigation
/*characters*  | forward search for a pattern which will take you to the next occurrence.
n  | for next match in forward
N | for previous match in backward
? | search for a pattern which will take you to the previous occurrence.
n | for next match in backward direction
N | for previous match in forward direction
CTRL+F | forward one window
CTRL+B | backward one window
CTRL+D | forward half window
CTRL+U | backward half window
j | navigate forward by one line
k | navigate backward by one line
G | go to the end of file
g | go to the start of file
q or ZZ | exit the less pager
10j | 10 lines forward.
10k | 10 lines backward.
CTRL+G | show the current file name along with line, byte and percentage statistics.
v | using the configured editor edit the current file.
h | summary of less commands
&pattern | display only the matching lines, not all.
ma | mark the current position with the letter ‘a’,
'a | go to the marked position ‘a’.
