# Tmux utilization

Tmux, like Screen, is a terminal multiplexer, a tool that allows you to use multiple terminals within a single display. It allows you to launch jobs/simulations/processes on a machine on which you have connected via ssh, and to terminate the ssh connection without killing the processes launched. This is very interesting to avoid losing long simulations in case of temporary network failure.


The prefix of the `tmux` keyboard shortcuts is ++ctrl+b++ followed by another key after releasing ++ctrl++.

!!! warning
    `tmux` shortcuts are done as follows:

    1. ++ctrl+b++
    2. release ++ctrl++
    3. press the key of the desired shortcut (++b++, ++c++ etc)

The shortcut to display all shortcuts: ++ctrl+b++ `?`

### Manipulate `tmux` sessions

| Action                               | Shortcut or command                                          |
|--------------------------------------|--------------------------------------------------------------|
| create a new session                 | `tmux`                                                       |
| create a new session by naming it    | `tmux new -s session_name`                                   |
| rename a session                     | `tmux rename -t old_name new_name`                           |
| detach from a session and let it run | ++ctrl+b++ then ++d++                                        |
| list existing sessions               | `tmux ls`                                                    |
| attach to the last session           | `tmux attach` or `tmux a`                                    |
| attach to the `session_name` session | `tmux a -t session_name`                                     |
| go to the next session               | ++ctrl+b++ then `)`                                          |
| go to the previous session           | ++ctrl+b++ then `(`                                          |
| kill a session                       | `tmux kill-session -t session_name` or close all its windows |


Once a session is open, the status line appears at the bottom of the terminal, indicating the name of the session, and the additional window(s) opened.


### Manipulate windows in sessions

It is possible to create windows within a session.

| Action                           | Shortcut or command   |
|----------------------------------|-----------------------|
| create a new window in a session | ++ctrl+b++ then ++c++ |
| navigate between windows         | ++ctrl+b++ then ++n++ |
| rename a window                  | ++ctrl+b++ then `,`   |
| go to next window                | ++ctrl+b++ then ++n++ |
| go to previous window            | ++ctrl+b++ then ++p++ |


### Example to launch a long simulation in a tmux session, keeping the homedir mount with krenew

Launch the simulation and disconnect:

- Connect via ssh to the desired server
- Open a `tmux` session: `tmux new -s mySimulation`
- Launch the simulation: `krenew -K 60 "python3 my-simulation.py --and --its --arguments"`
- Detach from the session: ++ctrl+b++ ++d++
- Disconnect from the server: `exit`

Return to your tmux session:

- Reconnect to the server
- Attach to your `tmux` session: `tmux a -t mySimulation`

Once the simulation is finished, type `exit` twice, once to end the `tmux` session and once to exit the ssh session.

### Switch to navigation mode (scroll) in the window

To be able to scroll up in a `tmux` window, you need to switch to `scroll` mode with the shortcut ++ctrl+b++ then `[` or ++f7++. To exit `scroll` mode and be able to enter a new command, type ++q++.

### Sources

- [https://doc.ubuntu-fr.org/tmux](https://doc.ubuntu-fr.org/tmux){target="_blank"}
- [https://cheatography.com/cloudranger/cheat-sheets/ultimate-tmux-v2-3/](https://cheatography.com/cloudranger/cheat-sheets/ultimate-tmux-v2-3/){target="_blank"}
