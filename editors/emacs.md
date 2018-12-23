# Emacs notes
## install
  * brew cask install emacs
  * brew install emacs --HEAD --with-cocoa --with-gnutls --with-librsvg --with-imagemagick@6 --with-mailutils 
  * [How can I install emacs correctly on OS X?](https://stackoverflow.com/a/47155790)
  * [Emacs For Mac OS](https://www.emacswiki.org/emacs/EmacsForMacOS)
  * [A Guided Tour of Emacs](http://www.gnu.org/software/emacs/tour/)
  * [The Emacs Editor](http://www.gnu.org/software/emacs/manual/html_node/emacs/index.html)
  * [Emacs Wiki](https://www.emacswiki.org/)
  * [MELPA repository](https://github.com/melpa/melpa)
  * Install `exec-path-from-shell` package
    * [exec-path-from-shell](https://github.com/purcell/exec-path-from-shell)
    ```lisp
    (when (memq window-system '(mac ns x))
    (exec-path-from-shell-initialize))
    ```
    * [Why Emacs overrides my PATH when it runs bash?](https://emacs.stackexchange.com/questions/14159/why-emacs-overrides-my-path-when-it-runs-bash)
## tips
1. `M+x shell RET` start zsh, control characters in prompt, edit .zshrc
  ```
  if [[ $TERM = dumb ]]; then
    unset zle_bracketed_paste
  else
    test -e "${HOME}/.iterm2_shell_integration.zsh" && source "${HOME}/.iterm2    _shell_integration.zsh"
  fi
  ```
  * [[Help] terminal zsh control characters in prompt](https://www.reddit.com/r/emacs/comments/5p3njk/help_terminal_zsh_control_characters_in_prompt/)
  * [zsh in Emacs output junk characters](https://stackoverflow.com/questions/7494461/zsh-in-emacs-output-junk-characters)
  * [weird characters in shell mode with zsh](https://emacs.stackexchange.com/questions/19848/weird-characters-in-shell-mode-with-zsh)
 
 2. How to select a region of text
   ```
   C-x SPC or C-x @
   down or up multiline
   C-t $text RET
   ```
   * [How to do select column then do editing in GNU Emacs?](https://superuser.com/questions/77314/how-to-do-select-column-then-do-editing-in-gnu-emacs)
   
