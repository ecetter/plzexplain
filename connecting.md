
# Connecting to Roar and Roar Collab

The most straightforward way to connect to Roar Collab (RC) is with a secure shell client via the `ssh` command. On MacOS or Linux devices, the default terminal application has an `ssh` command available by default. On Windows devices, a secure shell client, like [PuTTy](https://www.putty.org/), must be downloaded and installed.

The host address for RC is **submit.hpc.psu.edu**. To connect, an active Penn State access account user ID and password is required. Typically, port 22 is used for secure shell connections. A user without access to RC can request an account by submitting an [account request](https://www.icds.psu.edu/computing-services/account-setup/). For any external collaborators, a university faculty member must set up a sponsored access account with the university Accounts Office to provide access. 

From a terminal session, a connection can be made with RC using
```
$ ssh <userid>@submit.hpc.psu.edu
```

A password must be entered and then multi-factor authentication must be completed successfully to complete the login.


## Portal

Users may also connect to ICDS systems through the Portal powered by Open OnDemand. The [Roar Collab Portal](https://rcportal.hpc.psu.edu/) offers a graphical connection to ICDS systems and may be preferable for users that are uncomfortable in a command line environment.



