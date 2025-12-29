An ansible scrypt to remotely Unlock an Encrypted Linux Server Using Dropbear.<br />
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
For better maintainability and security, you can load the public key from a file on the Ansible control machine.
