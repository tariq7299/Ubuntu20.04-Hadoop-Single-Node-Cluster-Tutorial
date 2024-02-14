
1. **Modes**:
   - Vim operates in different modes:
     - **Normal mode**: Used for navigation and executing commands.
     - **Insert mode**: For typing and editing text.
     - **Visual mode**: Select text for manipulation.
   - Press **Esc** to switch to normal mode from any other mode.

2. **Basic Navigation**:
   - **h**: Move left
   - **j**: Move down
   - **k**: Move up
   - **l**: Move right
   - **w**: Jump to the next word
   - **b**: Jump back to the beginning of the word
   - **$**: Move to the end of the line
   - **0**: Move to the beginning of the line

3. **Editing Text**:
   - **i**: Enter insert mode before the cursor
   - **a**: Enter insert mode after the cursor
   - **o**: Open a new line below
   - **O**: Open a new line above
   - **x**: Delete the character under the cursor
   - **dd**: Delete the current line
   - **yy**: Yank (copy) the current line
   - **p**: Paste the yanked text

4. **Saving and Quitting**:
   - **:w**: Save changes
   - **:q**: Quit (close) Vim
   - **:wq**: Save and quit
   - **:q!**: Quit without saving

5. **Search and Replace**:
   - **/search_term**: Search for `search_term`
   - **:%s/old/new/g**: Replace all occurrences of `old` with `new`

6. **Customization**:
   - Create a `~/.vimrc` file to customize Vim settings.
   - Install plugins using a plugin manager like **Vundle** or **Pathogen**.

