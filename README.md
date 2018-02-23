# Remote VS Code

## Setup
1. Install 'Remote VSCode' extension for Visual Studio Code.
1. Check User Settings in VS Code (optional)
    ```sh
    "remote.host": "127.0.0.1"
    "remote.port": 52698
    "remote.onstartup": false
    "remote.dontShowPortAlreadyInUseError": false

    # NOTE: These are the defaults. They determine which address to listen on, the port to use, whether to launch the VS Code server on startup, and whether to show an error when trying to launch the server when it is already running.
    ```
1. Install rmate helper script on your server. This is a shell script to allow files to be edited on a remote server similar to TextMate's remote editing capability. We use the following command to download the script from the GitHub repo and put it in the /usr/local/bin/ folder. In your server, type the following command:

    <pre>

    <code>
    sudo wget -O /usr/local/bin/[rmate/rcode] https://raw.github.com/aurora/rmate/master/rmate

    # NOTE
    # In the server filepath, you might find it more logical to call it rcode instead of rmate (because you will be editing with VS Code instead of TextMate). Whatever you call it, that will be the name of the command you use when editing files. Notice the GitHub repo URL does not change.
    </code>
    
    </pre>

1. Make the command executable. The following code makes the helper script executable as a command. Use 'chmod a+x' to set all permission for you and your user group to allow the script to be executable. The filepath you give it will be the same as the filepath from the step above where you installed the script. Once this is done the command 'rcode' (or 'rmate', whatever you chose) will be a command you can run on the server.

    ```sh
    sudo chmod a+x /usr/local/bin/[rmate/rcode]
    ```


## Use
1. Launch VS Code's server. Type Ctrl/Cmd + Shift + P to open VS Code's command palette. If you start to type "Remote" you should see the command "Remote: Start Server". Selecting that will launch the server, which is listening on port 52698 (because that is the default setting when you install Remote VSCode).
1. Open an SSH tunnel. An SSH tunnel is a secure connection between a local computer and a remote server to forward a port. By doing this, we are connecting our server-side rcode/rmate command to our local machine's VS Code editor (which is now running its own server to listen for the connection). Run the following command from your local machine:
    ```sh
    ssh -NR 52698:localhost:52698 YOUR_SERVER

    # NOTE
    # -N is an option to prevent a new session from opening (i.e., you want to connect to the server without actually going into the server to work).
    # -R is an option to specify the direction of the ssh connection, starting remotely and ending on the local machine (-L would go the other direction).
    ```
1. Open the file in VS Code. Run the following command:
    ```sh
    [rmate/rcode] -w -p 52698 file

    # NOTES
    # You will not type any brackets. You will choose rmate or rcode depending on what you named the command. 
    # The '-p 52698' is optional since the port is already 52698 by default. 
    # The -w option is recommended, since it tells the server to wait for the file to be closed in VS Code and then stop the rmate/rcode process. 
    # As an example, if you rcode is your command, and the file you want to open is 'server/index.js', you would type 'rcode -w server/index.js'.
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

    # NOTE: You can use -ntpl4 to just get ipv4 processes, or -ntpl6 for just ipv6 processes.
    ```