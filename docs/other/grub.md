## Runlevels
To change the run level, press "e" in the grub menu, go to the line where starting with "kernel" and press "e" again, it'll go to the end of the line of the command add the number/name of the run level and press "Entre" then "b" to boot to the system

- 0 or single: Boot to the root user, allow you to change the passoword ..
- 3: Boot to non-graphical system
- init=/bin/bash: Bypass init and just drop me at a shell, nothing will start
