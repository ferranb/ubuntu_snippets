# How to connect to ssh without a password

**Server**: The remote session where you want to connect and run something.

**Client**: The local session where you are connecting from and don't want to use the password.

## Client steps (no server-side steps required)

Check if a key already exists, if you used the default file name proposed:

    ls ~/.ssh/id_rsa ~/.ssh/id_rsa.pub

If they do not exists, generate a new key and accept the default file name:

    ssh-keygen -t rsa -b 4096
    
For each server you want to connect to without a password, run:

    ssh-copy-id username@remotehostname

You’ll need to enter the password this one time only, of course.

Now you can connect without a password:

    ssh username@remotehostname

## Optional: Set a Default Username

Edit ` ~/.ssh/config` and add:

    Host remotehostname
        User username
Then you can simply run:

    ssh remotehostname

## More

If you're thinking about connecting without typing a password, username, or even a hostname... well, as far as I know, that's not possible. 

