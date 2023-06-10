
# ![[vim.png]] Registers

## Basic info

### Register Usage
In normal mode
```
"0 p
 will paste content of 0 register

"a yy
 will yank everything in current line to a register
```
In insert mode
```
Ctrl+r a
 will paste content of a register
```
Setting value of a register
```
:let @a=@_
  will set the a register equal to black hole register, ie make it empty
```

## Types of registers

1. <u>Unnamed register:</u> `vim` has a unnamed (or default) register that can be accessed with `""`. Any text that you delete (with `d`, `c`, `s` or `x`) or yank (with `y`) will be placed there, and that’s what `vim` uses to `p`aste, when no explicit register is given. A simple `p` is the same thing as doing `""p`.           
2. <u>Numbered registers:</u> `vim` automatically populates what is called the **numbered registers** for us. As expected, these are registers from `"0` to `"9`.  
   `"0` will always have the content of the latest yank, and the others will have last 9 deleted text, being `"1` the newest, and `"9` the oldest. So if you yanked some text, you can always refer to it using `"0p`.                                                                                                                                                     
3. <u>Named registered:</u> Named registers are represented by lowercase letters from a to z. To store text in a named register, you can use the "register" command, denoted by the double quote (`"`), followed by the register name and the yank or delete command. For example, `"a yy` yanks the current line into register `a`, while `"a dd` deletes the current line into register `a`.                                                                                                                                                    
4. <u>Read only registers:</u> There are 4 read only registers: `".`, `"%`, `":` and `"#` .The last inserted text is stored on `".`, and it’s quite handy if you need to write the same text twice, in different places, not needing to yank and paste.`"%` has the current file path, starting from the directory where `vim`was first opened. `":` is the most recently executed command. If you save the current buffer with `:w`, “w” will be in this register. A good way to use it is with `@:`, to execute this command again. For example, if you execute a substitute command in one line, like in `:s/foo/bar`, you can just to go another line and execute `@:` to run this substitution again. If you want to paste your last executed command in command box in command mode, you can press `ctrl+r:`                                                                                                                                                   
5. <u>The expression and the search registers:</u> The expression register (`"=`) is used to deal with results of expressions. This is easier to understand with an example. If, in insert mode, you type `Ctrl-r =`, you will see a “=” sign in the command line. Then if you type `2+2 <enter>`, `4` will be printed. This can be used to execute all sort of expressions, even calling external commands. To give another example, if you type `Ctrl-r =` and then, in the command line, `system('ls') <enter>`, the output of the `ls` command will be pasted in your buffer. The search register, as you may have imagined, is where the latest text that you searched with `/`, `?`, `*` or `#` is. If, for example, you just searched for `/Nietzsche`, and now you want to replace it with something else, there is no way you are going to type “Nietzsche” again, just do `:%s/<Ctrl-r />/mustache/g` and you are good to go.                                                                                                                                                                   
6.  <u>The black hole register:</u> represented by the underscore character (`"_`), it is a special register that discards any text yanked or deleted into. To delete text into the black hole register, you can use the "register" command (`"`) followed by the black hole register (`"_`) and the delete command. For example, `"_dd` deletes the current line and sends it to the black hole register, without affecting any other registers.
   For example, if you want to yank a line to register `a` and then delete that line, ie, not have the deleted text go into numbered registers, you can do `"ayy "_dd `


## Macros

You may already be familiar with `vim`’s macros. It’s a way to record a set of actions that can be executed multiple times (`:h recording` if you need more information). What you probably didn’t know is that `vim` uses a register to store these actions, so if you use `qw` to record a macro, the register `"w` will have all the things that you did, it’s all just plain text.

The cool thing about this is that, as it is just a normal register, you can manipulate it as you want. How many times have you forgotten that step in the middle of a macro recording and had to do it all over again? Well, fixing that is as simple as editing a register.

For example, if you forgot to add a semicolon in the end of that `w` macro, just do something like `:let @W='i;'`. Noticed the upcased `W`? That’s just how we append a value to a register, using its upcased name, so here we are just appending the command `i;` to the register, to enter insert mode (`i`) and add a semicolon. If you need to edit something in the middle of the register, just do `:let @w='<Ctrl-r w>`, change what you want, and close the quotes in the end. Done, no more recording a macro 10 times before you get it right.

Another cool thing about this is that, as it’s just plain text in a register, you can easily move macros around, applying it in other `vim` instance, or sharing it with someone else. Think about it, if you have that register in your clipboard, you can just execute it with `@+` (`"+` is the clipboard register). Try it, just write “ivim is awesome” anywhere, then copy it to your clipboard, and execute `@+` in a `vim` buffer.


