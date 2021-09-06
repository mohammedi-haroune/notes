### Capabilities
Special permissions, example: running chown.
`man capabilities` for more info.
> For  the  purpose  of performing permission checks, traditional UNIX implementations distinguish two categories of processes: privileged processes (whose effective user ID is 0, referred to as superuser or root), and unprivileged processes (whose effective UID is nonzero).  Privileged  processes  bypass  all  kernel  permission  checks,  while unprivileged processes are subject to full permission checking based on the process's credentials (usually: effective UID, effective GID, and supplementary group list).

> Starting with kernel 2.2, Linux divides the privileges traditionally associated with superuser into distinct units,  known  as  capabilities,    which can be independently enabled and disabled.  Capabilities are a per-thread attribute.

### Signals
- `SIGINT`: interrupt, tell it to kill itself, the process can catch it and shutdown gracefully, terminal sends it when hitting Ctrl-C
- `SIGTERM`: terminate, tell it to kill itself gracefully or not
- `SIGKILL`: kill, kill the process, the process can't catch it so it can't cleanup before being killed
- `SIGSTOP`: stop or pause the process, terminal sends it when hitting Ctrl-Z. The shell uses pausing (and its counterpart, resuming via SIGCONT) to implement job control

#### Relation
Now that we understand more about signals, we can see how they relate to each other.

The default action for SIGINT, SIGTERM, SIGQUIT, and SIGKILL is to terminate the process. However, SIGTERM, SIGQUIT, and SIGKILL are defined as signals to terminate the process, but SIGINT is defined as an interruption requested by the user.

In particular, if we send SIGINT (or press Ctrl+C) depending on the process and the situation it can behave differently. So, we shouldn’t depend solely on SIGINT to finish a process.

As SIGINT is intended as a signal sent by the user, usually the processes communicate with each other using other signals. For instance, a parent process usually sends SIGTERM to its children to terminate them, even if SIGINT has the same effect.

In the case of SIGQUIT, it generates a core dump which is useful for debugging.

Now that we have this in mind, we can see we should choose SIGTERM on top of SIGKILL to terminate a process. SIGTERM is the preferred way as the process has the chance to terminate gracefully.

As a process can override the default action for SIGINT, SIGTERM, and SIGQUIT, it can be the case that neither of them finishes the process. Also, if the process is hung it may not respond to any of those signals. In that case, we have SIGKILL as the last resort to terminate the process.


![](assets/ansible_architecture.png)
### Extra

- Root user can do everything, he lives in the kernel space and the normal capabilities, permissions ... don't apply to him

- /proc/sysrq-trigger: interface that gives access to the linux kernel, it allow us to communicate with kernel and tell him to do very low level stuff. Example

- Crash my system:
`echo c >/proc/sysrq-trigger`. You can use it to test the reliability of a high available system

-
