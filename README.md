# git-log-interactive

## Intro
This is `git log` for cool kids: [Quick Demo](https://www.loom.com/share/b2da0dfd2b9c4ffa96523017c9dd1fe1?sid=3bbe74ce-6c49-4927-b7de-490c5d41e069)

## Usage
1. Run in your terminal ```gli``` to invoke the function.
2. To do a fuzzy search, just start typing.
3. To view all changes in fullscreen, highlight a commit and press ```ENTER```, press `q` to exit fullscreen.
4. To copy a commit hash to your clipboard, press ```CTRL-C```.
5. To exit the function, press ```ESC```.

## Setup
> Note: This function uses [fzf](https://formulae.brew.sh/formula/fzf#default) and [diff-so-fancy](https://formulae.brew.sh/formula/diff-so-fancy#default), make sure they're both installed.

1. Navigate to your root directory with `cd ~`.
2. Open your `.zshrc` file with `nano .zshrc`.
3. Copy and paste the shell function at the bottom of the file:
```zsh
 function gli() {
   local git_log_line_to_hash="echo {} | grep -o '[a-f0-9]\{7\}' | head -1"
   local view_git_log_line="${git_log_line_to_hash} | xargs -I % sh -c 'git show --color=always % | diff-so-fancy'"

   git log \
       --color=always \
       --format="%C(cyan)%h %C(blue)%ar%C(auto)%d %C(yellow)%s%+b %C(black)%ae" "$@" | \
   fzf -i -e +s \
       --reverse \
       --tiebreak=index \
       --no-multi \
       --ansi \
       --preview="${view_git_log_line}" \
       --header "ENTER: View in pager, CTRL+C: Copy hash" \
       --bind "enter:execute:${view_git_log_line} | less -R" \
       --bind "ctrl-c:execute:${git_log_line_to_hash} | pbcopy"
 }
```
4. To save the file, press `CTRL-X`, then `Y`, then `ENTER`.
5. Apply the changes with `source ~/.zshrc`.

> Note: If you're using bash, follow the same steps but replace `.zshrc` with `.bashrc`.