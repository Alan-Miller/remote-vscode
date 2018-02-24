# Remote VS Code

## Overview
Do you hate Vim? Me, too. Are you one of the [one million developers who has had to ask for help to exit Vim?](https://stackoverflow.blog/2017/05/23/stack-overflow-helping-one-million-developers-exit-vim/) Me, too. Do you love VS Code and miss it every time you have to edit a file on your server? Not me, not anymore. Now I edit those remote files in VS Code and life is lovely.

## Setup
1. **Install 'Remote VSCode' extension for Visual Studio Code.**
1. **Check User Settings in VS Code (optional).**
    ```sh
    "remote.host": "127.0.0.1"
    "remote.port": 52698
    "remote.onstartup": false
    "remote.dontShowPortAlreadyInUseError": false

    # host: which address to listen on
    # port: which port to use
    # onstartup: whether to automatically launch the VS Code server when VS Code is started
    # dontShowPortAlreadyInUseError: whether to show an error if the server is already running
    ```
    NOTE: These are the defaults. I like the defaults.
1. **Install rmate helper script on your server.** This is a shell script to allow files to be edited on a remote server similar to TextMate's remote editing capability. We use the following command to download the script from the GitHub repo and put it in the /usr/local/bin/ folder. In your server, type the following command:
    ```sh
    sudo wget -O /usr/local/bin/[rmate/rcode] https://raw.github.com/aurora/rmate/master/rmate

    # Do not type the brackets. Call it rmate or rcode.
    ```
    NOTE: In the server filepath, you might find it logical to call it rcode instead of rmate (since you will be editing with VS Code, not TextMate). Whatever you call it, that will be the name of the command you use when editing files. The GitHub repo URL does not change.

1. **Make the command executable.** The following code makes the helper script executable as a command. Use 'chmod a+x' to set all permission for you and your user group to allow the script to be executable. The filepath you give it will be the same as the filepath from the step above where you installed the script. Once this is done the command 'rcode' (or 'rmate', whatever you chose) will be a command you can run on the server.

    ```sh
    sudo chmod a+x /usr/local/bin/[rmate/rcode]
    ```


## Use
1. **Launch VS Code's server.** Type Ctrl/Cmd + Shift + P to open VS Code's command palette. If you start to type "Remote" you should see the command "Remote: Start Server". Selecting that will launch the server, which is listening on port 52698 (because that is the default setting when you install Remote VSCode).
1. **Open an SSH tunnel.** An SSH tunnel is a secure connection between a local computer and a remote server to forward a port. By doing this, we are connecting our server-side rcode/rmate command to our local machine's VS Code editor (which is now running its own server to listen for the connection). Run the following command from your local machine:
    ```sh
    ssh -NR 52698:localhost:52698 YOUR_SERVER

    # -N prevents a new session (i.e., you want to connect without going into the server to work).
    # -R specifies direction, starting remotely and ending on local machine (opposite is -L)
    # '52698:localhost:52698' connects 52698 on the remote server to 52698 on your machine.
    ```
1. **Open the file in VS Code.** Run the following command:
    ```sh
    [rmate/rcode] -w file

    # You will not type brackets. You will choose the name you gave the command. 
    # You can specify port 52698 with '-p 52698', but it's already the default.
    # The -w is recommended. It waits for the edited file to be closed, then stops the server process. 
    # Example: rcode -w server/index.js
    ```



## Extra notes
- If you forget to use the -w option when editing a file, you may end up with multiple rmate/rcode processes running that you eventually have to kill. If so, use:
    ```sh
    sudo killall [rmate/rcode]
    ```

- To see processes that are running, use:
    ```sh
    ps aux | grep [rmate/rcode]
    ```

- To see node processes, use:
    ```sh
    netstat -ntpl | grep node

    # You can use -ntpl4 to just get ipv4 processes.
    # You can use -ntpl6 for just ipv6 processes.
    ```
