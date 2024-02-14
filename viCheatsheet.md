Sure, here's a basic `vi` cheat sheet:

**Entering and Exiting `vi`:**
```bash
vi filename  # Open 'filename' in vi
:w  # Save changes
:q  # Quit vi
:wq  # Save changes and quit vi
:q!  # Quit vi without saving changes
```

**Moving the Cursor:**
```bash
h  # Move left
j  # Move down
k  # Move up
l  # Move right
w  # Move to start of next word
b  # Move to start of previous word
0  # Move to start of line
$  # Move to end of line
G  # Move to end of file
gg  # Move to start of file
```

**Editing Text:**
```bash
i  # Insert text before the cursor
a  # Insert text after the cursor
o  # Open a new line below the current line
O  # Open a new line above the current line
x  # Delete the character under the cursor
dd  # Delete the current line
yy  # Copy the current line
p  # Paste the copied text after the cursor
P  # Paste the copied text before the cursor
u  # Undo the last operation
Ctrl + r  # Redo the last undo
```

**Searching Text:**
```bash
/string  # Search forward for 'string'
?string  # Search backward for 'string'
n  # Repeat the last search
N  # Repeat the last search in opposite direction
```

**Replacing Text:**
```bash
:r  # Replace the current line with the next line from the file
:1,10s/old/new/g  # Replace 'old' with 'new' in lines 1 to 10
:%s/old/new/g  # Replace 'old' with 'new' in the entire file
```
