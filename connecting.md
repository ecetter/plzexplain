
# Connecting to Roar and Roar Collab

The most straightforward way to connect to Roar and Roar Collab is to with a secure shell client via the **ssh** command. On MacOS or Linux devices, the default terminal application has an **ssh** command available by default. On Windows devices, a secure shell client, [PuTTy](https://www.putty.org/), must be downloaded and installed.

The host address for Roar is **submit.aci.ics.psu.edu** and for Roar Collab is **submit.hpc.psu.edu**. To connect, an active Penn State access account user ID and password is required. Typically, port 22 is used for secure shell connections.

From a terminal session, a connection can be made with Roar Collab using
```
$ ssh <userid>@submit.hpc.psu.edu
```
and with Roar using
```
$ ssh <userid>@submit.aci.ics.psu.edu
```

A password must be entered and then a multi-factor authentication method must be selected and completed successfully.


## Portal

Users may also connect to ICDS systems through the Portal powered by Open OnDemand. The [Roar Collab Portal](https://rcportal.hpc.psu.edu/) and the [Roar Portal](https://portal2.aci.ics.psu.edu/) offer a graphical connection to ICDS systems and may be preferred for users that are uncomfortable in a command line environment.

