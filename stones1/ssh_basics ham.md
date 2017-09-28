# Secure Shell

### Secure Shell

Secure Shell aka SSH, is a remote network protocol that allows users to control and modify their remote servers over the Internet. The service was created as a secure replacement for the unencrypted Telnet and uses cryptographic techniques to ensure that all communication to and from the remote server happens in an encrypted manner. In addition it provides  a mechanism for authenticating a remote user, supports tunneling, forwarding arbitrary TCP ports and X11 connections; file transfer can be accomplished using the associated SFTP or SCP protocols.

Any Linux or macOS user can SSH into their remote server directly from the terminal window. Windows users can take advantage of SSH clients like Putty.  You can execute shell commands in the same manner as you would if you were physically operating the remote computer.

## Opening an SSH Session

If you’re using Linux or Mac, then using SSH is very simple. If you use Windows, you will need to utilize an SSH client to open SSH connections. The most popular SSH client is PuTTY

For Mac and Linux users, head over to your terminal program and then follow the procedure below:

The SSH command consists of 3 distinct parts:

```ssh {username}@{serverip}     ```

The SSH key command instructs your system that you want to open an encrypted Secure Shell Connection. {username} represents the account you want to access. For example, you may want to access the root user, which is basically synonymous for system administrator with complete rights to modify anything on the system. {host} refers to the computer you want to access. This can be an IP Address (e.g. 192.30.252.154) or a domain name (e.g. www.moycyber.io).

When you hit enter, you will be prompted to enter the password for the requested account. When you type it in, nothing will appear on the screen, but your password is, in fact being transmitted. Once you’re done typing, hit enter once again. If your password is correct, you will be greeted with a remote terminal window.

The first time around it will ask you if you wish to add the remote host to a list of known_hosts, go ahead and say yes.

```The authenticity of host 'mocyber.io (192.30.252.154)' can't be established.
RSA key fingerprint is 53:b4:ad:c8:51:17:99:4b:c9:08:ac:c1:b6:05:71:9b.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mocyber.io' (RSA) to the list of known hosts. ```

If the server only allows public-key authentication, see **About SSH keys**

## Basic SSH Commands


```SSH Command```	Explanation

```ls```	Show directory contents (list the names of files).

```cd```	Change Directory.

```mkdir```	Create a new folder (directory)

```touch```	Create a new file

```rm```      Remove a file.

```cat```	Show contents of a file.

```pwd```	Show current directory (full path to where you are right now).

```cp```	Copy file/folder.

```mv```	Move file/folder.

```grep```	Search for a specific phrase in file/lines.

```find```	Search files and directories.

```vi/nano```	Text editors.

```history```	Show last 50 used commands.

```clear```	Clear the terminal screen.

```tar```	Create & Unpack compressed archives.

```wget```	Download files from the internet.

```du```	Get file size.

## Logging into a Server with a Different Port

By default the SSH daemon on a server runs on port 22. Your SSH client will assume that this is the case when trying to connect. If your SSH server is listening on a non-standard port (this is demonstrated in a later section), you will have to specify the new port number when connecting with your client.

You can do this by specifying the port number with the ``` -p ```option:

```ssh -p port_num username@remote_host```

To avoid having to do this every time you log into your remote server, you can create or edit a configuration file in the ~/.ssh directory within the home directory of your local computer.

Edit or create the file now by typing:

```nano ~/.ssh/config ```

In here, you can set host-specific configuration options. To specify your new port, use a format like this:

```

Host remote_alias
    HostName remote_host
    Port port_num ```

This will allow you to log in without specifying the specific port number on the command line.

## About SSH Keys

SSH keys provide a more secure way of logging into a server with SSH than using a password alone. One immediate advantage this method has over traditional password authentication is that you can be authenticated by the server without ever having to send your password over the network. Anyone eavesdropping on your connection will not be able to intercept and crack your password because it is never actually transmitted. Additionally, using SSH keys for authentication virtually eliminates the risk posed by brute-force password attacks by drastically reducing the chances of the attacker correctly guessing the proper credentials. Generating a key pair provides you with two long string of characters: a public and a private key. You can place the public key on any server, and then unlock it by connecting to it with a client that already has the private key. When the two match up, the system unlocks without the need for a password. You can increase security even more by protecting the private key with

To authenticate using SSH keys, a user must have an SSH key pair on their local computer. On the remote server, the public key must be copied to a file within the user's home directory at ``` ~/.ssh/authorized_keys ```. This file contains a list of public keys, one-per-line, that are authorized to log into this account.

### Step One—Create the RSA Key Pair
SSH keys are 2048 bits by default. This is generally considered to be good enough for security.

``` ssh-keygen -t rsa ```

You can specify a greater number of bits for a more hardened key.
To do this, include the ```-b``` argument with the number of bits you would like. Most servers support keys with a length of at least 4096 bits. Longer keys may not be accepted for DDOS protection purposes:

``` ssh-keygen -b 4096```


If you had previously created a different key, you will be asked if you wish to overwrite your previous key:

```Overwrite (y/n)? ```

If you choose "yes", your previous key will be overwritten and you will no longer be able to log into servers using that key. Because of this, be sure to overwrite keys with caution.

### Step Two—Store the Keys and Passphrase
Once you have entered the Gen Key command, you will get a few more questions:

``` Enter file in which to save the key (/home/demo/.ssh/id_rsa): ```

This prompt allows you to choose the location to store your RSA private key. Press ENTER to leave this as the default, which will store them in the .ssh hidden directory in your user's home directory. Leaving the default location selected will allow your SSH client to find the keys automatically.

You can press enter here, saving the file to the user home (in this case, my example user is called demo).

```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:```

It's up to you whether you want to use a passphrase. Entering a passphrase does have its benefits: the security of a key, no matter how encrypted, still depends on the fact that it is not visible to anyone else. Should a passphrase-protected private key fall into an unauthorized users possession, they will be unable to log in to its associated accounts until they figure out the passphrase, buying the hacked user some extra time. The only downside, of course, to having a passphrase, is then having to type it in each time you use the Key Pair.

If you choose to enter a passphrase, nothing will be displayed as you type. This is a security precaution.

The entire key generation process looks like this:

```

ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/demo/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/demo/.ssh/id_rsa.
Your public key has been saved in /home/demo/.ssh/id_rsa.pub.
The key fingerprint is:
4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 demo@a
The key's randomart image is:
+--[ RSA 2048]----+
|          .oo.   |
|         .  o.E  |
|        + .  o   |
|     . = = .     |
|      = S = .    |
|     o + = +     |
|      . o + o .  |
|           . o   |
|                 |
+-----------------+
```

The public key is now located in /home/demo/.ssh/id_rsa.pub The private key (identification) is now located in /home/demo/.ssh/id_rsa


### Step Three—Copy the Public Key
Once the key pair is generated, it's time to place the public key on the server that we want to use.

You can copy the public key into the new machine's authorized_keys file with the ssh-copy-id command. Make sure to replace the example username and IP address below.

```ssh-copy-id user@123.45.56.78```

Alternatively, you can paste in the keys using SSH:

```cat ~/.ssh/id_rsa.pub | ssh user@123.45.56.78 "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys" ```

No matter which command you chose, you should see something like:

```
The authenticity of host '12.34.56.78 (12.34.56.78)' can't be established.
RSA key fingerprint is b1:2d:33:67:ce:35:4d:5f:f3:a8:cd:c0:c4:48:86:12.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '12.34.56.78' (RSA) to the list of known hosts.
user@12.34.56.78's password:
Now try logging into the machine, with "ssh 'user@12.34.56.78'", and check in:

  ~/.ssh/authorized_keys

to make sure we haven't added extra keys that you weren't expecting. ```

Now you can go ahead and log into user@12.34.56.78 and you will not be prompted for a password. However, if you set a passphrase, you will be asked to enter the passphrase at that time (and whenever else you log in in the future).


# Here's a configuration file with a label:

```nginx
[label /etc/nginx/sites-available/default]
server {
    listen 80 default_server;
    . . .
}
```




![Alt text for screen readers](http://mocyber.io/uploads/8/9/7/8/89787785/mocyber_1.png?516)



## Conclusion
