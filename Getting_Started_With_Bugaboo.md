# Getting Started with Bugaboo and the Command Line

**Ryan Melnyk, Haney Lab, UBC**  
**Sept/Oct 2016**  
***ryan.melnyk@msl.ubc.ca***

WestGrid is a collection of servers across academic institutions in western Canada.
There are a lot of [options](https://www.westgrid.ca/support/systems) but the Haney
lab primarily uses ***bugaboo*** for computing and sharing data. This is because it has a bunch of bioinformatics
software already installed, it's very large in terms of CPUs and storage, and it's optimized for general computing tasks.

Before getting started, make sure that you've followed all of the steps in the
WestGrid_Onboarding document, and you've gotten a response from WestGrid support
indicating that you've been added to the "wg-haney" UNIX user group.

## Logging into bugaboo for the first time

This is intended for people running Mac OS X on their local computer. Also assumes that there's a working understanding
of navigating directories and opening files using the Terminal. Check out
[this intro](https://computers.tutsplus.com/tutorials/navigating-the-terminal-a-gentle-introduction--mac-3855) which covers all you need to know to get started.

+ open up the Terminal app
+ enter the following command

```bash
ssh yourname@bugaboo.westgrid.ca
```
+ For security purposes, on your first time connecting, you will be warned that you are connecting to an unknown host.
+ Type "yes" and bugaboo will be added to a list of known hosts.
+ You're on bugaboo! Every time you log-in, you will be in your home directory.
+ Use ``ls`` to see what's in your home directory. You should see two files which are actually "symlinks";
these are convenient links to other places in the filesystem. Think of them as shortcuts.
+ Remove the data link by using the command ``rm data``.  We won't need this link for our setup.
+ Ok, now exit bugaboo by using the ``exit`` command.

##### RTFM

There's good online documentation for bugaboo. See [here](https://www.westgrid.ca/support/quickstart/bugaboo). Particularly pay attention to
the information on storage and running jobs. WestGrid is an awesome resource and we want to be diligent users. If you're unsure of anything, or want more in-depth explanation please come and talk to me.

##### (Optional) Adding aliases for common commands.

Typing "ssh melnyk@bugaboo.westgrid.ca" or other common commands can get super tedious after a while. Most command line users
will make aliases for these common commands.

Run the following commands:

```bash
echo "alias bug='ssh melnyk@bugaboo.westgrid.ca'" >> ~/.bash_profile
source ~/.bash_profile
```

The ``echo`` command adds the line to the This adds an alias to the ``.bash_profile`` text file which is a file that gets loaded every time you start up. It's located in the ``~``
folder, which itself is an alias for your home folder (``/Users/yourname``). The ``source`` command then re-loads the
``.bash_profile``, now with your edits.

Now you can type in ``bug`` at the command prompt and you will be able to log-in to bugaboo more easily. Try it!


## Sharing files with labmates on bugaboo using the wg-haney group

##### Modifications to .bash_profile

We will now add a couple of lines to your ``.bash_profile`` file on the bugaboo server. Running the following commands will improve the aesthetics of the command prompt and your ``ls`` command.

```
echo "export CLICOLOR=1" >> ~/.bash_profile
echo "export LSCOLORS=gxfxbEaEBxxEhEhBaDaCaD" >> ~/.bash_profile
echo "alias ls='ls --color'" >> ~/.bash_profile
source ~/.bash_profile
```

##### Test if you are in the wg-haney group

On bugaboo, use the ``groups`` command. If you've been added to the ``wg-haney`` group you should see the following output.

```bash
melnyk@bugaboo:~$ groups
melnyk ubc wg-haney
```

This is a list of groups you are a member. By default, you should be part of a single-user group containing only yourself (``melnyk`` in my case),
a group for sharing data within UBC (``ubc``), and if you've got it setup with WestGrid support, our lab group (``wg-haney``).

Being in the ``wg-haney`` group means that you have permission to see what's in everybody else's home folder. For example, running the command
``ls /home/melnyk`` will let you see what's in my folder. Give it a shot - you should be able to see the directories and files in my home folder. Anybody outside of ``wg-haney`` would get a "permission denied" error message trying to do this.


##### Understanding file ownership and group associations

For any given file there is a file owner and an associated group. Running ``ls -l -h`` will show you this information. Let's try it on my home folder:

```
melnyk@bugaboo:~$ ls -l -h /home/melnyk
total 188K
drwxr-x---  2 melnyk wg-haney 4.0K Sep 23 12:16 adapters
drwxr-x--- 14 melnyk wg-haney 4.0K Sep 26 23:22 assemblies
drwxr-x---  2 melnyk wg-haney 4.0K Sep 23 12:25 bin
-rw-r-----  1 melnyk melnyk    288 Sep 22 18:12 generate_strainlist.py
drwxr-x---  3 melnyk melnyk   4.0K Sep 25 11:55 GenomeAssembly
drwxr-x---  3 melnyk melnyk   4.0K Sep 26 23:13 genomeDB_toolkit
drwxr-x---  6 melnyk melnyk   4.0K Sep 23 12:25 installs
-rw-r-----  1 melnyk melnyk    101 Sep 30 13:21 i.pbs
drwxr-x---  2 melnyk melnyk   4.0K Sep 22 18:07 miscrippets
drwxr-x---  6 melnyk melnyk   4.0K Sep 22 17:10 phylophlan
drwxr-x---  3 melnyk melnyk   4.0K Sep 22 21:42 PyParanoid
lrwxrwxrwx  1 melnyk melnyk     22 Sep 20 22:51 scratch -> /global/scratch/melnyk
drwxr-x---  2 melnyk wg-haney 4.0K Sep 30 10:01 shared_labdata
```

Ok. So this is a list of folders and files in my home directory. I own all of them, as indicated by the ``melnyk`` in the third column.
The fourth column is the group owner. You should be able to view all of the folders that are associated with the ``wg-haney`` group but
you will be denied access to the ones in the ``melnyk`` group. Try it!

##### More explanation on UNIX permissions

The first column in the ``ls -l -h`` output contains information about specific permissions for each file or folder. This info is displayed in the following format:

![permissions](http://i.stack.imgur.com/I0ei5.jpg)

So for the following line:

```
drwxr-x--- 14 melnyk wg-haney 4.0K Sep 26 23:22 assemblies
```

"d" means it is a directory, the first "rwx" means that the file owner (``melnyk``) has read-write-execute access, the second "r-x" means that
everyone in the associated group (``wg-haney``) has read-execute access, and the final "---" means that there are no global permissions.

By default, every folder you create you will have "read-write-execute" permissions and the associated group will have "read-execute" permissions. The execute permission
is needed to list the files in the folder. This is why you can view the contents of folders owned by ``wg-haney`` but not ``melnyk`` for example. By default, folders and files
have no global permissions and 99% of the time should stay that way.  Remember that WestGrid is used by thousands of different users across several provinces - we don't want to
share our data necessarily with everybody who uses bugaboo.

Text files (like scripts, for example) will have "read-write" permission for you and "read" permissions for group members.

Permissions can be changed using the ``chmod`` command, but make sure you know what you're doing before using it. You are only allowed to change permissions when you are the owner.

One common command you may need to use is ``chmod ug+x script.sh`` to allow execution of a script you've written. By default, you can't
execute text files so if you write a bash script, you need to change the permission to allow it to be executed. This command basically
says: Modify permissions (``chmod``) for the user owner (``u``) and the associated group (``g``) to allow execution (``+x``) of ``script.sh``.

##### Sharing your files

By convention, I'm recommending that we all make a folder called ``shared_labdata`` in our home folders to store and share raw data.

Run the following commands on bugaboo:

```
chgrp wg-haney /home/yourname
mkdir ~/shared_labdata
chgrp -R wg-haney /home/yourname/shared_labdata
```

Note that everytime you add something new to ``shared_labdata`` you will have to run ``chgrp -R wg-haney /home/yourname/shared_labdata`` to add
everything in the folder to ``wg-haney``.

## Downloading and uploading files to bugaboo.

The ``scp`` (secure copy) command can be used on both your mac and on bugaboo to "push" or "pull" data back and forth. Usage is relatively straightforward. The first argument is the file to be copied and the second argument is the destination. Note the format for connecting to a remote server. Use the ``-R`` argument to copy entire folders (R = Recursive)

```
scp rawreads.fa melnyk@bugaboo-fs.westgrid.ca:rawreads
# The path is relative to the home directory - this would copy to ~/rawreads.

scp -R assembly_folder melnyk@bugaboo-fs.westgrid.ca:/global/scratch/melnyk
# or you can use absolute path.

scp melnyk@bugaboo-fs.westrgrid.ca:genomedb.tre .
# retrieving a file and copying to local directory
```

##### FTP Client (FileZilla)

Alternatively, you can use an FTP client. I recommend FileZilla.  This integrates file browsers to simplify the process as well as queues which can come in handy for copying large datasets.  Use ``bugaboo-fs.westgrid.ca`` as the host, your credentials and Port 22, then hit "QuickConnect".

***THIS PART IN PROGRESS***
