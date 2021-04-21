# tmux

Part of the Red Primer series, learn to use tmux!

[tmux](https://tryhackme.com/room/rptmux)

## Topic's

* Linux Tools

## Screens wishes it was this cool.

![](https://i.imgur.com/GYktm26.png)

tmux, the terminal multiplexer, is easily one of the most used tools by the Linux community (and not just pentesters!). While not a malicious tool, tmux makes running simultaneous tasks throughout a pentest incredibly easy. In this primer room, we'll walk through the process of installing and using some of the most common key combinations used in tmux. (Note, the installation process in this is geared towards Kali/Ubuntu.)

![](https://i.imgur.com/bL9Dn3U.png)

Link to the above cheat sheet: [Link](https://imgur.com/bL9Dn3U)

Original credit for the cheat sheet goes to [Linux Academy](https://linuxacademy.com/blog/linux/tmux-cheat-sheet/)

For another excellent resource on learning tmux, check out IppSec's video: [Link](https://www.youtube.com/watch?v=Lqehvpe_djs)

Enjoy the room! For future rooms and write-ups, follow [@darkstar7471](https://twitter.com/darkstar7471) on Twitter.

* [Tmux Cheat Sheet & Quick Reference](https://tmuxcheatsheet.com/)

1. First things first, let's go ahead and install tmux. This can be done on Ubuntu/Kali with the command: apt-get install tmux

`No answer needed`

2. Once tmux is installed, let's launch a new session. What command do we use to launch a new session without a custom name?

* `tmux`
* `tmux new`
* `tmux new-session`
* `new`

> Start a new session

`tmux`

1. All tmux commands start with a keyboard button combination. What is the first key in this combination?

* `Ctrl`

`Control`

2. How about the second key? Note, these keys must be pressed at the same time and released before pressing the next target key in the combination.

* `Ctrl + b`

`b`

3. Lets go ahead and detach from our newly created tmux session. What key do we need to add to the combo in order to detach?

* `Ctrl + b d`

> Detach from session

`d`

4. Well shoot, we've detached from our session. How do we list all of our sessions?

* `tmux ls`
* `tmux list-sessions`
* `Ctrl + b s`

> Show all sessions

`tmux ls`

1. What did our session name default to when we created one without a set name?

```
tmux ls
0: 1 windows (created Sat Sep 26 17:52:04 2020) (attached)
```

`0`

2. Now that we've found the name of our session, how do we attach to it?

* `tmux a -t mysession`
* `tmux at -t mysession`
* `tmux attach -t mysession`
* `tmux attach-session -t mysession`

> Attach to a session with the name mysession

`tmux a -t 0`

3.  Let's go ahead and make a new window in this session. What key do we add to the combo in order to do this?

* `Ctrl + b c`

> Create window

`c`

4.  Seems like we have plenty of windows and nothing to fill them up with. Let's remedy that problem by deploying the VM above and running and nmap scan against it. Deploy the VM now.

`No answer needed`

11. Run the following scan against the VM: nmap -sV -vv -sC TARGET_IP

`No answer needed`

12. Whew! Plenty of output to work with now! If you work with a relatively small terminal like me, this output might not all fit on screen at once. To fix that, let's enter 'copy mode'. What key do we add to the combo to enter copy mode?

* `Ctrl + b [`

> Enter copy mode

`[`

13. Copy mode is very similar to 'less' and allows up to scroll up and down using the arrow keys. What if we want to go up to the very top?

* `g`

> Go to top line

`g`

14. How about the bottom?

* `G`

> Go to bottom line

`G`

1.  What key do we press to exit 'copy mode'?

* `q`

> Quit mode

`q`

1.  This window we're working in is nice and all but I think we need an upgrade. What key do we add to the combo to split the window vertically?

* `Ctrl + b %`

> Split pane vertically

2.  How about horizontally?

* `Ctrl + b "`

> Split pane horizontally

3.  We can now move between these panes using the key combo and arrow keys, try it out!

`No answer needed`

19. We can also resize these panes by holding down the key combo and pressing the arrow keys, try it out now!

`No answer needed`

20. Wait a minute, we've forgotten about our original window! We can go back it using the key combo and the number of the session! Try going back to this original window and then returning to our new one!

`No answer needed`

21. Say one of these newly minted panes becomes unresponsive or we're just done working in it, what key do we add to the combo to 'kill' the pane?

* `Ctrl + b x`

> Close current pane

22. Now that's we've finished out work, what can we type to close the session? 

`exit`

23. Last but now least, how do we spawn a named tmux session named 'neat'?

* `tmux new -s mysession`
* `new -s mysession`

> Start a new session with the name mysession

`tmux new -s neat`