# How to connect to ssh with X11 forwarding (i.e., forwarding the display)

**Server**: The remote session. Where you want to connect and run something.
**Client**: The local session. Where you want the graphical interface to appear.

## Server Requirements

Install required packages (probably they are installed):

    sudo apt install openssh-client install xauth xdg-utils

Ensure `/etc/ssh/sshd_config` contains the following line:

    X11Forwarding yes

You can check it with:

    egrep '^X11Forwarding +yes' /etc/ssh/sshd_config

Restar the server is you have changed it:

    sudo systemctl restart ssh

## Client Requirements (probably fulfilled)

Install required packages (probably they are installed, too):

    sudo apt install openssh-client xauth xdg-utils

## Test it 

Run:

    ssh -Y username@remotehost 

And execute any graphic application

You might see messages like:

    X11 connection rejected because of wrong authentication

But it usually works fine anyway.

## If `sudo` breaks the display forwarding

Run the command with environment forwarding:

    sudo -E command 

## Optionall: Avoid typing -Y every time

Edit (or create) the file `~/.ssh/config` and add:

    Host *
      ForwardX11 yes
      ForwardX11Trusted yes

