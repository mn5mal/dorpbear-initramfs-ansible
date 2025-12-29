## Remotely Unlock an Encrypted Linux Server Using Dropbear

This Ansible playbook enables remote unlocking of a LUKS-encrypted Linux server during the early boot (initramfs) stage using Dropbear SSH.

**Tested on:**
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)

To allow SSH access during the initramfs stage, you must provide at least one SSH public key.<br />
Dropbear will use this key for authentication before the root filesystem is mounted.<br />
You should place your public ssh key in this section:<br />
<pre>
  - name: Add dropbear authorized_keys
      copy:
        dest: /etc/dropbear/initramfs/authorized_keys
        mode: 0600
        owner: root
        group: root
        content: |
          public-ssh keys
</pre><br />
- For better maintainability and security, you can load the public key from a file on the Ansible control machine.<br /><br />
Example login:
<pre>$ ssh -i ~/.ssh/unlock_remote -p 2222 -o "HostKeyAlgorithms ssh-rsa" root@192.168.1.23
Enter passphrase for key '/home/foo/.ssh/unlock_remote': 
To unlock root partition, and maybe others like swap, run `cryptroot-unlock`.


BusyBox v1.35.0 (Debian 1:1.35.0-4+b3) built-in shell (ash)
Enter 'help' for a list of built-in commands.

~ # cryptroot-unlock 
Please unlock disk sda3_crypt: 
cryptsetup: sda3_crypt set up successfully
~ # Connection to 192.168.1.23 closed by remote host.
Connection to 192.168.1.23 closed.</pre>
