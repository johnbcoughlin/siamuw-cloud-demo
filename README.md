# siamuw-cloud-demo

# Useful Commands

## Command line basics

- `cd`: Move between directories.
  - `cd -` will return to the previous directory
  - `cd ..` goes to the parent directory
  - `cd ~` goes to your home directory.
  - `cd` with no arguments is equivalent to `cd ~`
- `ls`: Have a look-see at directory contents.
  - `ls -a` will show "hidden" files, whose names start with a `.`
  - `ls -latr` will show the flags and last-modified times of all the files in a
    directory, sorted.
- *Tab completion*: many commands support tab completion. For example, typing
  `cd` then the first letters of a directory name, followed by tab, will tab
  complete the intended directory. Try it!
- `man <CMD>` brings up the manual for many UNIX standard tools.
- All processes have two streams, STDIN and STDOUT, which can be "connected"
  with a pipe. For example, `ls | wc -l` will count the number files in the
  current directory, by piping the "list" from `ls` into the word count utility.

## Working with text

- `cat`: "Concatenate" the contents of a file to the shell. Will display the
  entire file!
- `less`: Enter a scrolling file viewer. The viewer can be navigated using some
  different key commands, inspired by `vim`:
  - `/` to search forward
  - `G` to go to the very end
  - `d` and `u` to go down and up
  - `gg` to go all the way to the top
  - `?` to search backwards (sometimes useful)
- `head` and `tail` show the beginning and ending lines of a file.
  - `head -n <N>` shows the first `N` lines, likewise for tail.
- `wc`: Word count.
  - `wc -l` to count lines.
- `grep`: Searches text.
  
## Working with processes

- `Ctrl-C`: Kill the currently running process. If you only know one thing about
  the command line, it should be this.
- `htop`: Show CPU and memory usage for the whole machine.
- `ps aux`: List all the processes in a useful format.
  - I'm always running `ps aux | grep <commandname>` to find processes.
- `screen`: A very useful command which is somewhat intimidating. Type `screen`
  to open a new "screen session". Then start a long-running command, like a ML
  training job or something. Carefully type `<ctrl>-A D` (press Control A, then
  let up on Control and press D) to "detach" from the screen session.
- `renice`: Make a process that you started "nice", like: `renice +10 <PID>`.
  Get the PID from `htop` or `ps aux`.
  
# Networking

- `wget`: Downloads files.
- `scp`: "Secure copy"--part of the SSH family of commands. Use like this: 
  - To upload a file: `scp <file> <user>@<remotehost>:/path/to/destination`
  - To download a file to the current directory (namely, `.`): `scp
    <user>@<remotehost>:/path/to/file .`

## SSH

- `ssh-keygen`: Create an SSH key, by default `~/.ssh/id_rsa[.pub]`. The public
  key is the one with the `.pub` suffix. To transmit these, you can just copy
  them to your clipboard. You should never need to move or give
  out the private key (the one without a suffix).
- `ssh -NL <port>:localhost:<port> <user>@<remotehost>`: Causes connections made
  /from/ your machine to the URL `localhost:<port>` to be /forwarded/ to the
  remote host. This is extremely useful for running servers such as Jupyter on a
  remote host, where you can take advantage of processing power, while still
  using a friendly interface like the jupyter notebook.

# Python Environments

The recommended way to manage your own python environment on an AMATH server is
with Miniconda. Install it like so:
```
wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.9.2-Linux-x86_64.sh
chmod +x Miniconda3-py39_4.9.2-Linux-x86_64.sh
./Miniconda3-py39_4.9.2-Linux-x86_64.sh
```

Now create an environment with:
```
conda create -n <environmentName>
conda activate <environmentName>
```

# Setting up a Jupyter Notebook remotely

Install Jupyter from within your conda environment:
```
conda install -c conda-forge notebook
conda install -c anaconda ipykernel
python -m ipykernel install --user --name=<environmentName>
```

In a screen session,
```
screen
jupyter notebook --no-browser --port=<someRandomPort>
Ctrl-A D # to detach the screen session
```

On your local computer:
```
ssh -NL <port>:localhost:<port> <user>@<remoteHost>.amath.washington.edu
```
