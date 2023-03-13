
# Transferring Files

Data manager nodes are available on Roar, so use the data manager hostname of **datamgr.aci.ics.psu.edu** to transfer file to/from Roar. Roar Collab does not have data manager nodes, so the submit nodes should be used instead (hostname **submit.hpc.psu.edu**). When specifying a file location, it is best to use the full file path.


## scp

It is recommended to use the **scp** command to transfer files to and from Roar and Roar Collab. It is typically practical to zip files together when transferring many files at once. In a terminal session, the **scp** command can be used to transfer files in the following way:
```
$ scp [options] <source-user-id>@<source-host>[:<file-location>] <destination-user-id>@<destination-host>[:<file-location>]
```

Some examples will make the usage more clear. The two locations in this example are the directory **/home/abc** on a local laptop device and ICDS scratch spaces. If a file named *local.file* in **/home/abc** is to be transferred to a user's scratch space, the user should run the following command from a terminal session on the local laptop:
```
# Transfer to Roar scratch space
$ scp /home/abc/local.file <userid>@datamgr.aci.ics.psu.edu:/gpfs/scratch/<userid>

# Transfer to Roar Collab scratch space
$ scp /home/abc/local.file <userid>@submit.hpc.psu.edu:/scratch/<userid>
```

Alternatively, if the user navigates to the **/home/abc** location on their local laptop, the commands can be slightly simplified to
```
# Transfer to Roar scratch space
$ scp local.file <userid>@datamgr.aci.ics.psu.edu:/gpfs/scratch/<userid>

# Transfer to Roar Collab scratch space
$ scp local.file <userid>@submit.hpc.psu.edu:/scratch/<userid>
```

If a directory named *datadir* is located on the user's ICDS scratch space is to be transferred to the local laptop's **/home/abc** directory, the user can run the following from a terminal session on the local laptop:
```
# Transfer from Roar scratch space
$ scp -r <userid>@datamgr.aci.ics.psu.edu:/gpfs/scratch/<userid> /home/abc/

# Transfer from Roar Collab scratch space
$ scp -r <userid>@submit.hpc.psu.edu:/scratch/<userid> /home/abc/
```

Note that since a directory is being transferred, the **-r** option must be used for the **scp** command so the directory and its contents are transferred. If the user navigates the terminal to the **/home/abc** directory to conduct the transfer, then the **/home/abc/** in the above commands can be replaced with a single period **.** to denote the *current working directory*.
```
# Transfer from Roar scratch space
$ scp -r <userid>@datamgr.aci.ics.psu.edu:/gpfs/scratch/<userid> .

# Transfer from Roar Collab scratch space
$ scp -r <userid>@submit.hpc.psu.edu:/scratch/<userid> .
```


## sftp

The **sftp** command can also be used to transfer files and is more useful when transferring multiple smaller files in a piecemeal fashion. This method allows for interactive navigation on the remote connection. From a local device, an **sftp** connection can be made with
```
$ sftp <userid>@<remote-host>[:<location>]
```

To spawn an **sftp** connection on Roar, use
```
$ sftp <userid>@datamgr.aci.ics.psu.edu
```
And on Roar Collab, use
```
$ sftp <userid>@submit.hpc.psu.edu
```

After the **sftp** connection is made, navigational commands (i.e. **ls**, **cd**, etc) are performed on the remote connection normally, while navigational commands are performed on the local connection by appending the letter **l** (lowercase L) to the commands (i.e. **lls**, **lcd**, etc). Files are transferred from the local device to the remote device using the **put <filename>** command, and files are transferred from the remote device to the local device using the **get <filename>** command. The connection is terminated with the **exit** command.


## rsync

Yet another file transfer option is **rsync**. The **rsync** tool is widely used for backups and mirroring and as an improved cophy command for everyday use. The **rsync** command takes the form
```
$ rsync [options] <source-user-id>@<source-host>[:<file-location>] <destination-user-id>@<destination-host>[:<file-location>]
```


## Transfers Using Globus

For files that are multiple GB in size or larger, it is recommended to use Globus when practical to do so. Globus is a file transfer tool that automates the activity of managing file transfers, such as montitoring performance, retrying failed transfers, recovering from faults automatically whenever possible, and reporting status.

Globus endpoints must be install on both the source and destination systems. Both Roar and Roar Collab have Globus endpoints available.
```
Roar Endpoints:
PennState_ICS-ACI_DTN_EndPnt_01
PennState_ICS-ACI_DTN_EndPnt_02
PennState_ICS-ACI_DTN_EndPnt_03

Roar Collab Endpoints:
PennState_ICDS_RC
```

To transfer files with Globus, visit the [Globus website](https://www.globus.org/) and log in as a Penn State user with your Penn State access account. Select the File Manager tab on the left side of the Globus web interface and select the source and destination endpoints. The endpoints may require an additional login. The files and locations for the transfer can be selected graphically, and the transfer can be initiated by selecting *Start* above the source endpoint file preview window. The transfer will be handled by Globus, and typically, successful completion of the transfer will generate an email to your Penn State email.