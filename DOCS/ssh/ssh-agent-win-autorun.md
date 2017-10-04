The result: use PuTTY + ssh-pageant.

>1. 'First solution' Assumes Windows, msysgit, and PuTTY.
>2. ssh-pageant is a tiny tool for Windows that allows you to use SSH keys from PuTTY's Pageant in Cygwin and MSYS shell environments.

---
https://stackoverflow.com/questions/370030/why-git-cant-remember-my-passphrase-under-windows

Here is two step-by-step solutions, depending on whether you use TortoiseGit in addition to msysgit or not.

- [x] 'First solution' Assumes Windows, msysgit, and PuTTY.

1. Install msysgit and PuTTY as instructed.
2.(Optional) Add PuTTY to your path.<br>
  (If you do not do this, then any references to PuTTY commands below must be prefixed with the full path to the appropriate executable.)<br>
3. If you have not done so already, then generate a key hash as instructed at GitHub or as instructed by your Git host.
4. Again, if you have not already done so, convert your key for use with PuTTY's pageant.exe using puttygen.exe.<br>
   Instructions are in PuTTY's documentation, in this [helpful guide](https://www.electrictoolbox.com/putty-rsa-dsa-keys/), and several other places in cyberspace.
5. Run PuTTY's pageant.exe, open your .ppk file ("Add Key"), and provide your passphrase for your key.
6. Access Windows' environment variables dialog<br>
  (Right-click on "Computer", Click on "Properties", Click on "Advanced system settings" or the "Advanced" tab, click on "Environment Variables").<br>
   Add the following environment variable:
   
   ``` GIT_SSH=C:\full\path\to\plink.exe ```

   Replace "C:\full\path\to" with the full installation path to PuTTY, where plink.exe is found.<br>
   It is probably best to add it to the "User variables" section.<br>
   Also, make sure that the path you use to plink.exe matches the path you use for Pageant (pageant.exe).<br>
   In some cases you may have several installations of PuTTY because it might be installed along with other applications.<br>
   Using plink.exe from one installation and pageant.exe from another will likely cause you trouble.

7. Open a command prompt.
8. If you are trying to connect to a git repository hosted at Github.com then run the following command:

   ``` plink.exe git@github.com ```

   If the git repository you are trying to connect to is hosted somewhere else, then replace git@github.com with an appropriate user name and URL.<br>
   For e.g., plink.exe -p 22 ivansoft@github.com<br>
  (Assuming Github) You should be informed that the server's host key is not cached, and asked if you trust it.
   Answer with a ```'y'```.<br>
   This will add the server's host key to PuTTY's list of known hosts.<br>
   Without this step git commands will not work properly.<br>
   After hitting enter, Github informs you that Github does not provide shell access.<br>
   That's fine...we don't need it.<br>
  (If you are connecting to some other host, and it gives you shell access, it is probably best to terminate the link without doing anything else.)

9. All done! Git commands should now work from the command line.<br>
   You may want to have pageant.exe [load your .ppk file automatically at boot time](http://blog.shvetsov.com/2010/03/making-pageant-automatically-load-keys.html), depending on how often you'll be needing it.

- [ ] 'Second solution' Assumes Windows, msysgit, and TortoiseGit.

  no-no, only without TortoiseGit please

- [ ] 'Third solution' Assumes Window, msysgit, and native command prompt.

1. Install msysgit
2. Make sure to allow git to be used on the MS-DOS command prompt
3. Run start-ssh-agent
4. Enter SSH passphrases
5. All done! Git commands should now work in the native command prompt.

https://github.com/cuviper/ssh-pageant#ssh-pageant

An SSH authentication agent for Cygwin/MSYS that links OpenSSH to PuTTY's Pageant.<br>
ssh-pageant is a tiny tool for Windows that allows you to use SSH keys from PuTTY's Pageant in Cygwin and MSYS shell environments.

You can use ssh-pageant to automate SSH connections from those shells, which is useful for services built on top of SSH, like SFTP file transfers or pushing to secure git repositories.

ssh-pageant works like ssh-agent, except that it leaves the key storage to PuTTY's Pageant. It sets up an authentication socket and prints the environment variables, which allows OpenSSH connections to use it.

---
