# How to connect to ssh with X11 forwarding (i.e., forwarding the display)

**Server**: The remote session where you want to connect and run something.

**Client**: The local session where you want the graphical interface to appear.

## Server Requirements

Install required packages (they might already be installed):

    sudo apt install openssh-server xauth xdg-utils

Ensure `/etc/ssh/sshd_config` contains the following line:

    X11Forwarding yes

You can check it with:

    egrep '^X11Forwarding +yes' /etc/ssh/sshd_config
    
Restart the SSH server if you made changes:

    sudo systemctl restart ssh

## Client Requirements

Install required packages (they're likely already installed):

    sudo apt install openssh-client xauth xdg-utils

## Test it 

Connect with:

    ssh -Y username@remotehost 

And execute any graphic application. For instance, `xeyes`.

## If `sudo` breaks the display forwarding

You might see messages like:

    X11 connection rejected because of wrong authentication
    
Run the command with environment forwarding:

    sudo -E command 

## Optional: Avoid typing -Y every time

Edit (or create) the file `~/.ssh/config` and add:

    Host *
      ForwardX11 yes
      ForwardX11Trusted yes

