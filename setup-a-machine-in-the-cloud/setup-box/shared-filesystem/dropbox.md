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
    curl -Lo dropbox-linux-x86_64.tar.gz https://www.dropbox.com/download?plat=lnx.x86_64 && \
    sudo mkdir -p /opt/dropbox && \
    sudo tar xzfv dropbox-linux-x86_64.tar.gz --strip 1 -C /opt/dropbox && \
    /opt/dropbox/dropboxd
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
    1. Leave the terminal window with `dropboxd` running in the foreground, as-is.
        * NOTE: `dropboxd` will start syncing everything in your dropbox account immediately, the next few steps will work to correct this behaviour.
1. Open a new `ssh` session with your remote machine and download the `dropbox.py` command-line tool so we may tell `dropboxd` to avoid syncing anything other than our code/projects.

    ```
    mkdir -p ~/bin && \
    wget -O ~/bin/dropbox.py "http://www.dropbox.com/download?dl=packages/dropbox.py" && \
    chmod 755 ~/bin/dropbox.py && \
    ~/bin/dropbox.py help
    ```
1. Switch to your `Dropbox` directory and exclude any files that it has begun syncing: 
    ```
    cd ~/Dropbox/ && ~/bin/dropbox.py exclude add *
    ```
1. Check the status: `~/bin/dropbox.py status`
    1. If its still trying to sync then every few minutes, rerun: `cd ~/Dropbox/ && ~/bin/dropbox.py exclude add *`
    1. Keep running the status check again too: `~/bin/dropbox.py status`
    1. After the status check declares: `Up to date`
        * Run this command one last time to be sure: `cd ~/Dropbox/ && ~/bin/dropbox.py exclude add *`
    1. Now the contents of your `Dropbox` directory should be empty: `cd ~/Dropbox/ && ls`
1. Now that we've excluded everything known to us, you can place any new code-related projects & folders into dropbox and they will sync between.
    * https://www.digitalocean.com/community/questions/dropbox-works-with-digitalocean-droplets
        > You probably don't want to host content out of your Dropbox folder, but there's a basic trick for mirroring Dropbox content outside of the Dropbox folder: create a symbolic link.

        > Example: ln -s /var/www/foo.com ~/Dropbox/foo.com

        > This will cause Dropbox to treat /var/www/foo.com as if it resided inside the Dropbox folder. All other clients will see foo.com as a folder within the Dropbox folder, and will transparently sync across that symlink.
1. We cannot count on keeping our first ssh session alive forever to run `dropboxd` so let's [set it up as a service](https://www.digitalocean.com/community/tutorials/how-to-install-dropbox-client-as-a-service-on-ubuntu-14-04#set-up-service-script)

        ```
    cd ~ && \
    sudo curl -o /etc/init.d/dropbox https://gist.githubusercontent.com/thisismitch/d0133d91452585ae2adc/raw/699e7909bdae922201b8069fde3011bbf2062048/dropbox && \
    sudo chmod +x /etc/init.d/dropbox
    ```
1. tbd

#### Other References
* https://github.com/jolantis/digitalocean-droplet-install-guide#setup-dropbox-sync