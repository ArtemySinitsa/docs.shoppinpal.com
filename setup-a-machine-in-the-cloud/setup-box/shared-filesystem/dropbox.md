# Setup a machine in the cloud

## Setup box

### Shared FileSystem

#### Dropbox

1. We can use dropbox for efficient 2-way project sync!
    * This approach works for any IDE - `atom` or `webstorm` or `vscode` - they don't have to know about the remote filesystem.
    * This will allow us to write code locally and have the changes show up on the remote box.
        * Adding/deleting files will work
        * File permissions will be properly reflected
        * Configuring `selective sync` via the dropbox UI will prevent the transfer of bulky dependencies back to your local filesystem
        * such features are often difficult to setup or absent in other sync tools.
    * Tools like `SourceTree` can simply deal with our local filesystem without any knowledge of the remote filesystem.
1. Install Dropbox agent
    * https://www.dropbox.com/install-linux
    * for digitalocean 64-bit droplets with Ubuntu OS:

    ```
    cd ~ && \
    wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf - && \
    ~/.dropbox-dist/dropboxd
    ```
1. Link the machine by copy/pasting the URL from the output window into your browser. Here's a sample of what you'll see:

    ```
    This client is not linked to any account...
    Please visit https://www.dropbox.com/cli_link?host_id=xxx
    to link this machine.
    ```
1. Once you see the following output:

    ```
    This computer is now linked to Dropbox. Welcome xxx
    ```
    Use `ctrl+c` to terminate the `dropboxd` process otherwise it will start syncing everything in your dropbox account immediately, which will create problems if your remote machine doesn't have enough space!
    * If you are slow to take action, do not worry, this can be easily corrected ... but the important thing is that you stop it sooner rather than later.
1. Download the `dropbox.py` command-line tool so we may tell `dropboxd` to avoid syncing anything other than our code/projects.

    ```
    mkdir -p ~/bin && \
    wget -O ~/bin/dropbox.py "http://www.dropbox.com/download?dl=packages/dropbox.py" && \
    chmod 755 ~/bin/dropbox.py && \
    ~/bin/dropbox.py help
    ```
1. blah


#### Other References
* https://github.com/jolantis/digitalocean-droplet-install-guide#setup-dropbox-sync