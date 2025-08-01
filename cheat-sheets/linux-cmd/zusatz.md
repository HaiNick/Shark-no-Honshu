# additional linux utilities for power users. example: better alternatives to standard tools, specialized disk/network monitoring

## disk usage and file analysis
```bash
# ncdu - interactive disk usage analyzer
ncdu /                             # analyze root filesystem
ncdu /home                         # analyze home directory
ncdu -x /                          # stay on same filesystem
ncdu -q /var                       # quiet mode, suppress warnings
ncdu -r /tmp                       # read-only mode

# duff - duplicate file finder
duff -r /home/user                 # find duplicates recursively
duff -e0 /home | xargs -0 rm       # find and delete duplicates
duff /media/backup /media/current  # compare two directories
```

## advanced text search
```bash
# ripgrep (rg) - fast text search
rg "pattern"                       # search current directory
rg -i "pattern"                    # case insensitive
rg -w "word"                       # whole word only
rg -v "pattern"                    # invert match
rg -l "pattern"                    # only filenames
rg -A 3 -B 3 "pattern"            # 3 lines context before/after
rg --type py "def "                # search only python files
rg -z "pattern"                    # search compressed files

# fd - modern find alternative
fd filename                        # find files by name
fd -t f "\.py$"                   # find files by extension
fd -t d config                     # find directories only
fd -H "\.env"                     # include hidden files
fd -x chmod 644 {}                 # execute command on results
```

## system monitoring tools
```bash
# glances - comprehensive system monitor
glances                           # interactive system monitor
glances -t 2                      # update every 2 seconds
glances --export csv              # export to csv
glances -s                        # server mode
glances -c server_ip              # client mode

# btop/htop alternatives
btop                              # modern system monitor  
bpytop                            # python version of bashtop
```

## network monitoring
```bash
# bandwhich - display current network utilization
bandwhich                         # show network usage by process
sudo bandwhich                    # requires sudo for full info

# dog - dns lookup tool (dig alternative)
dog google.com                    # simple dns lookup
dog @8.8.8.8 google.com          # use specific dns server
dog google.com MX                 # query mx records

# httpie - user-friendly http client
http GET httpbin.org/get          # get request
http POST httpbin.org/post name=john  # post with data
http --download httpbin.org/image/png  # download file
```

## file operations
```bash
# exa - modern ls alternative
exa -la                           # detailed listing
exa --tree                        # tree view
exa --git                         # show git status
exa -la --sort=modified           # sort by modification time

# bat - cat with syntax highlighting
bat file.py                       # syntax highlighted output
bat -n file.txt                   # show line numbers
bat --paging=never file.txt       # no paging

# fzf - fuzzy finder
fzf                               # interactive file selection
history | fzf                     # fuzzy search history
cat file.txt | fzf               # fuzzy search file contents
find . -type f | fzf | xargs vim  # find and edit file
```

## process management
```bash
# procs - modern ps alternative
procs                             # show all processes
procs firefox                     # filter by name
procs --tree                      # tree view
procs --watch                     # watch mode

# dust - disk usage analyzer
dust                              # analyze current directory
dust /home                        # analyze specific directory
dust -d 2                         # limit depth to 2 levels
```

## compression and archives
```bash
# ouch - unified compression tool
ouch compress files.tar.gz file1 file2  # create archive
ouch decompress archive.tar.gz    # extract any format
ouch list archive.zip             # list contents

# zoxide - smart cd replacement
z directory_name                  # jump to directory by name
z -l                              # list frequent directories
zi                                # interactive directory selection
```

## json and structured data
```bash
# jq - json processor
cat data.json | jq '.'            # pretty print json
cat data.json | jq '.users[0]'    # select first user
cat data.json | jq '.[] | .name'  # extract all names
cat data.json | jq 'map(select(.active))' # filter active items

# xsv - csv toolkit
xsv headers data.csv              # show column headers
xsv select name,age data.csv      # select columns
xsv search -s name "john" data.csv # search in column
xsv stats data.csv                # basic statistics
```

## terminal multiplexers
```bash
# tmux - terminal multiplexer
tmux new -s session_name          # create named session
tmux ls                           # list sessions
tmux attach -t session_name       # attach to session
tmux kill-session -t session_name # kill session

# tmux key bindings (default prefix: Ctrl+b)
# Ctrl+b, c    - create new window
# Ctrl+b, %    - split vertically
# Ctrl+b, "    - split horizontally
# Ctrl+b, d    - detach session
```

## system utilities
```bash
# hyperfine - command benchmarking
hyperfine 'command1' 'command2'   # benchmark commands
hyperfine --warmup 3 'grep pattern file' # with warmup runs

# tokei - code statistics
tokei                             # count lines of code
tokei --sort lines                # sort by line count
tokei src/                        # analyze specific directory

# delta - git diff viewer
git diff | delta                  # better diff output
git log -p | delta                # better log with diffs
```

## modern shell tools
```bash
# starship - modern shell prompt
# install and add to .bashrc/.zshrc
eval "$(starship init bash)"

# zsh-autosuggestions - command suggestions
# install via package manager or oh-my-zsh

# fish shell - user-friendly shell
fish                              # start fish shell
fish_config                       # web-based configuration
```

## installation commands
```bash
# cargo (rust) tools
cargo install ripgrep fd-find exa bat procs dust tokei hyperfine

# snap packages
snap install btop
snap install dog

# apt/debian packages
apt install ncdu duff glances fzf tmux jq

# pip packages
pip install glances bpytop

# npm packages (some tools)
npm install -g http-server fkill-cli
```

## configuration files
```bash
# common config locations
~/.bashrc                         # bash configuration
~/.zshrc                          # zsh configuration
~/.vimrc                          # vim configuration
~/.tmux.conf                      # tmux configuration
~/.gitconfig                      # git configuration
~/.ssh/config                     # ssh configuration
```

# TODO: add container tools (podman, buildah), kubernetes utilities (kubectl, helm), cloud cli tools