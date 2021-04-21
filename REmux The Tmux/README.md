# REmux The Tmux

Updated, how to use tmux guide. Defaults and customize your workflow.

[REmux The Tmux](https://tryhackme.com/room/tmuxremux)

## Topic's

* Linux Tools

## Task 1 Tmux practice machine

![](https://i.imgur.com/XzPGg47.png)

Tmux is known as a terminal multiplexer. That allows you to craft a single terminal however you need it.

﻿Here is a machine you can use to complete the room if you don't have tmux installed on your local machine. Also comes with all the code and plugins needed for future tasks.

Username: **tux** Password: **tmuxwithremux**

1. Start the VM if you need it and ssh in.

`No answer needed`

## Task 2 Starting tmux "Sessions" and default tmux "prefix"

To Start a new tmux session just run the tmux command with no arguments. The first session create will have the name of "0". By default tmux status bar will be green. With session name on the left. Windows in the middle and window names in the middle. Hostname, time, and date on the right of the bottom green bar.

![](https://i.imgur.com/oSA9qGE.png)

Tmux doesn't allow to create a nested tmux within a tmux unless you force it to. When running the tmux command a second time.

![](https://i.imgur.com/Y4EPE2M.png)

To change the session name from "0" -> "box-dev". Must first learn how tmux is called. All commands within a tmux session all start with the tmux prefix is. By default, the tmux prefix is "Ctrl b".

After the tmux prefix. To the hotkeys to change the current tmux session's name is "shift $". 

**ctrl b shift $**

Retype the new name and then enter-key to save the new session name.

![](https://i.imgur.com/qHQj6k3.png)

![](https://i.imgur.com/HfPNVag.png)

If there is a need to create another tmux session within the current one. Use the -d argument with the tmux command. To spawn a new tmux session without attaching to it. In the example image below. The -s argument is used to specify the session name for the new session. Typed as "tmux new -s <new-session-name> -d".

![](https://i.imgur.com/ERMg52k.png)

To list all active tmux sessions. Run tmux with list-sessions. Or the short version of list-sessions as ls. In the example below. Running tmux ls also shows the current session in use. Marked by "(attached)".

![](https://i.imgur.com/i3WCJhc.png)

Exiting a tmux session without closing it can be done with the prefix. Followed by d

**ctrl b d**

Checking again with the tmux ls command. "(attached)" is missing from both sessions. This means the sessions are active but we are detached and are unable to interact with either session.

![](https://i.imgur.com/Xhd2eev.png)

To reattach to a active tmux session. Run tmux with the attach option and -t followed by the desired session name.

![](https://i.imgur.com/jFraRG2.png)

The tmux session name has changed to the attached session of "tryhackme". Double-checking with tmux ls. Can confirm that "(attached)" also on the "tryhackme" session name.

![](https://i.imgur.com/B4vt59w.png)

Delete a single session by its session name. Is done with the kill-session option with tmux. Followed by -t and the <target-session-name-to-delete>

![](https://i.imgur.com/umE6QNw.png)

Listing sessions with tmux ls. Shows that the session name of box-dev has been removed. WARNING! By deleting the session. Anything open in that session will be lost if not saved before the tmux kill-session.

![](https://i.imgur.com/Nz8BXM0.png)

In this example below there are many sessions open. Another way to swap sessions without having to detach and reattach to another session. Is to use the prefix. Followed by the s-key to list all open sessions. Using up or down arrow keys to navigate to the desired tmux session. Then enter to select the new session.

**ctrl b s**

![](https://i.imgur.com/hYjo7Wh.png)

Change from session name "0" to "tmux_remux" without having to leave the current tmux session.

![](https://i.imgur.com/JlTFQYk.png)

![](https://i.imgur.com/mxNqdUk.png)

From the five active sessions above. If there was a need to kill all the sessions except for a single one. When using the tmux kill-session. Use the -a argument close all sessions except the one specified by the -t argument. Closing all tmux sessions except for the one named "is-0day-0k?". Checked with the tmux list-sessions.

![](https://i.imgur.com/OnDPvRh.png)

When spiting the session into different sized panes ("which will be covered in more detail in a later task"). The new pane will spawn in the directory that the tmux session was first stared in. 

![](https://i.imgur.com/10jcuPu.png)

To change the base starting directory. Must first learn about tmux prompt or command mode. The tmux prompt allows tmux sessions to run tmux commands without the tmux binary name. Useful when the terminal has been filled with other text. Enter a tmux prompt with prefix shift :

**ctrl b shift** :

Followed by "attach -c /path/to/new/starting/directory"

![](https://i.imgur.com/WBJEHcn.png)

With the updated starting or base directory done above as /dev/shm. Creating a new pane start in the /dev/shm directory.

![](https://i.imgur.com/aFTBDnj.png)

Even though at the start tmux doesn't allow nested tmux within a tmux. Attaching or starting a new tmux session on another computer by an ssh connection. Can make a nested tmux. Not a problem. Just by changing the number of prefix used before the following command. Can determine which session gets the command.

prefix, prefix, and command. This will run on the second nested tmux session of the ssh ubuntu machine.

prefix and command. This will run on the first tmux session. The session running on the current machine's localhost.

![](https://i.imgur.com/6O1gjXv.png)

1. Do the ctrl and b keys need to be held down the whole time with every commands to work? yea/nay

`nay`

2. How to start tmux with the session with the name "thm"?

`tmux new -s thm`

3. How to change the current tmux session name?

`ctrl b shift $`

4. How to quit a tmux session without closing the session? To attach back later.

`ctrl b s`

5. How to list all tmux sessions?

`tmux ls`

6. How to reattach to a detached tmux session with the session name of "thm"

`tmux a -t thm`

7. How to create a new tmux session from your current tmux session with the name kali?
8. How to switch between two or more tmux sessions without detaching from the current tmux session?

`ctrl b s`

9.  How do you force kill the tmux session named "thm" if it's not responsive from a new terminal window or tmux session?

`tmux kill-session -t thm`

10. Within a nested tmux session. A second tmux session within the first one. How to change the session name of the second/internal tmux session?
11. How to get into a tmux prompt to run/type tmux commands?
12. Are there than one way to exit a tmux prompt? yea/nay

`yea`

13. Is tmux case sensitive. Will hitting the caps lock break tmux? yea/nay

`yea`

14. Within tmux prompt or command mode how would you change the tmux directory? Where a new window or pane will start from the changed directory of /opt.
15. How to kill all tmux sessions accept the one currently in use? With the name "notes".

## Task 3 Manage tmux "Panes"

Within the current tmux session window. Tmux panes are used to divide the current session into multiple sized terminals. That allows multiple commands to run within the same session window.

To split the currently selected pane horizontally. Do the prefix. Followed by shift "

**ctrl b shift "**

![](https://i.imgur.com/X1zl13j.png)

To split the currently select pane vertically. Do the prefix. Followed by shift %

**ctrl b shift %**

![](https://i.imgur.com/360xQ6y.png)

The exit command can be used to close the currently selected pane.

![](https://i.imgur.com/fV3HHSA.png)

Moving to another pane with in the same window can be done with the prefix. Followed by the arrow keys. 

**ctrl b <arrow key>**

However, the main problem with this method is sometimes when using the arrow keys again. For example, to move the terminal cursor box could move to another tmux pane instead. 

To resolve this issue can prefix followed by o instead. This can cycle through all the tmux panes. This has the added benefit of the line cursor not escaping to another pane.

**ctrl b o**

To swap between the most used tmux panes if more than two panes are open. "ctrl b ;" is the better option. "ctrl b o" is better when there are just two panes open.

**ctrl b ;**

With <arrow-keys> and or <o> in the image below to move the terminal cursor box from the left to the.

![](https://i.imgur.com/bVOqU9g.png)

If a tmux pane isn't responsive and ctrl-c isn't resolving the issue. Force close or kill the currently selected tmux pane with the prefix and x. Then y to confirm.

**ctrl b x y**

![](https://i.imgur.com/o1VzEAM.png)

If killing the pane was a mistake. Use the n option for no instead of y for yes when the kill-pane prompt is shown.

![](https://i.imgur.com/4E3V4XQ.png)

![](https://i.imgur.com/92ANwXr.png)

Managing the placement of panes. Can change the layout without having to exit panes or open create new panes.

To move the currently selected pane. In a clockwise rotation. Do prefix shift }

**ctrl b shift }**

![](https://i.imgur.com/FW2wcmF.png)

Note. All other panes will move clockwise with the currently selected pane.

![](https://i.imgur.com/9Qs0N23.png)

To move the currently selected pane counter-clockwise. Do prefix shift {

**ctrl b shift {**

![](https://i.imgur.com/FW2wcmF.png)

Another way to manage the pane location is with five built-in layouts. This can be done with the prefix followed by escape OR esc key and the desired option from the number selected from 1 to 5.

**ctrl b esc 4**

![](https://i.imgur.com/9qhhbo2.png)

Note that the layout result depends on how many panes are currently open.

To cycle through the built-in pane layouts one at a time. Do the prefix with the spacebar. This can also be an alternative using a clockwise or counter-clockwise option when there are only two panes open on the current tmux window.

**ctrl b spacebar**

![](https://i.imgur.com/bIBBAIQ.png)

Lastly when it comes to managing panes. There might be a need to swap the place between two panes. When cycling panes don't create the desired pane layout.

![](https://i.imgur.com/rlcK0bW.png)

Before swapping, panes can be done. Must first check the number each pane as been assigned. This also provides the size of each pane.

![](https://i.imgur.com/CUcHEnU.png)

For the swap pane example. It will be done within tmux prompt or command mode with prefix shift : then the commands swap-pane. With -s <source-pane> and -t <destination-pane>. This will send the move/swap the currently selected pane and keep the currently selected pane selected after the swap.

**:swap-pane -s <source pane final swap location> -t <currently selected destination pane>**

![](https://i.imgur.com/aIdDnTg.png)

With the command above in this example. The currently selected pane of the parrot has moved to the bottom right and have followed the currently selected pane. Still able to start typing within the parrot pane without having to switch to another pane.

![](https://i.imgur.com/buv3O2E.png)

To swap places with another pane and select the other open pane. Instead of the currently selected pane. Have the -t <destination-pane> first with the pane you want to swap with. Followed by the -s <source-pane> or currently selected pane

**:swap-pane -t <pane to swap with> -s <currently selected pane>**

![](https://i.imgur.com/7xKfyJK.png)

![](https://i.imgur.com/q2lOeYY.png)

Started with the parrot in the bottom right pane. After the command. Have swapped places with the cow. Also, the new currently selected pane is the cow.

![](https://i.imgur.com/sHi4yOK.png)

1. How to create a new pane split horizontally?

`ctrl b shift "`

2. How to close a tmux pane like closing a ssh session?
3. How to create a new pane split vertically?

`ctrl b shift %`

4. How to cycle between tmux pre built layout options? Starting with the number 1.
5. How to cycle/toggle between tmux layouts, one at a time?
6. How to force quit a frozen, crashed or borked pane?

`ctrl b x y`

7. How to move between the two must used tmux panes for the current tmux window?

`ctrl b ;`

8. Can you use the arrow to move to the desired pane? yea/nay

`yea`

9.  How to move the currently selected pane clockwise?

`ctrl b shift {`

10. How to move the currently selected pane counter-clockwise

`ctrl b shift }`

11. Before using swap-pane. How to check for which pane has what number?
12. How to swap two panes and move with the swapped pane?  Within tmux prompt mode. 1 -> 3 location
13. How to swap two panes without changing the currently selected pane location? Within tmux prompt mode. 1 -> 4 pane number

## Task 4 Manage tmux "Windows"

Tmux windows are like a new terminal tab that you can easy swap from and more. To create a new empty tmux window to work on. Do prefix c. Shown with the new window number and name. Note the currently selected window will be marked with a * or wildcard star. At the end of the current window name.

**ctrl b c**

![](https://i.imgur.com/egHvr61.png)

To change the name of the current window. Do prefix ,. After retyping the new window name. Hit the enter key to save changes.

**ctrl b ,**

![](https://i.imgur.com/Rye7brq.png)

![](https://i.imgur.com/H128qzD.png)

Oh NO. The recon window has been cluttered with multiple panes and can't view the Nmap output. To detach a pane into its own window. move the desired pane to move to its own window. Then prefix shift !.

**ctrl b shift !**

![](https://imgur.com/LRlp0iP.png)

The Nmap has been sent to a new window named "bash". Now able to view the whole Nmap scan without run the Nmap scan again.

![](https://i.imgur.com/2dSyKWE.png)

To cycle between windows can be done with prefix n for the next window. Or prefix p for the previous windows.

Another method to switch to the desired window is to use prefix w. This will list all the tmux windows. Using the arrow keys to select the desired windows and enter-key to select the highlighted window.

![](https://imgur.com/VDJMLyG.png)

Moving back to the recon window. Shows the nmap pane is has been moved and there is more room to work.

![](https://imgur.com/DKWKE4p.png)

If a tmux window as been borked and needs to be terminated quickly. Killing the window will also close any panes open within the currently selected tmux window. To kill or close a tmux window do prefix shift & and followed by y to close the window. Or n to keep it open.

**ctrl b shift &**

![](https://imgur.com/Vh0CB0n.png)

Prompt after the command.

![](https://imgur.com/6jVDw4K.png)

Closing the recon window. The name recon is removed from the status bar below.

![](https://imgur.com/jQ18ddb.png)

Previously in this task was shown how to break off a pane into its own window. However, sometimes the is a need to fuse two windows together as a single window. 

To join two different windows/panes back into one. Can be done with the window name or number. The command is run within tmux prompt or command mode. Prefix shift : then within the prompt :join-pane -t <window-name> OR :join-pane -s <window-name>. Where -s is the source name and -t is the destination window name.

*First. Enter tmux prompt or command mode from the tmux session.*

**ctrl b shift :**

![](https://imgur.com/uWzG7Ll.png)

*Second. Type join-pane with the desired option and window name.*

**join-pane -s <source-window-name>**

**join-pane -t <destination-window-name>**

![](https://imgur.com/hzJwwCv.png)

*Third. Hit Enter to run the join-pane command.*

![](https://imgur.com/ZiklfoC.png)

For the join-pane commands adding -v on the end fuses the two panes together horizontally. Adding -h on the end of the join-pane command fuses two panes together vertically.

![](https://imgur.com/TsNDi7X.png)

![](https://imgur.com/5191Ob9.png)

1. How to create a new empty tmux window?

`ctrl b c`

2. How to change the currently select window's name?

`ctrl b ,`

3. How to move the currently selected pane to it's own tmux window?

`ctrl b shift !`

4. How to fuse two panes together with the "source" window of "bash"? After entering a tmux prompt?

`join-pane -s bash`

5. How to fuse two panes together with the "destination" window of "sudo"? After entering a tmux prompt?

`join-pane -t sudo`

6. What option can added with question 4 and 5 to fuse together vertically?

`-v`

7. What option can added with question 4 and 5 to fuse together horizontally?

`-h`

8.  With join-pane can you use the window number instead of the window's name? yea/nay

`yea`

9.  How to kill or completely close a window. Including all the panes open on that window. If it's unresponsive?

`ctrl b shift &`

10. How to view and cycle between all the tmux windows for the current tmux session without detaching from the current session?
11. How to move back to the previous tmux window?
12. How to move up to the next tmux window?

## Task 5 Tmux "copy" mode
## Task 6 Oh My Tmux and beyond
## Task 7 Oreo's open-source .tmux.conf file 