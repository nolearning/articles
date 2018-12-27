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

## Basic Operation
* Move
  C-p, C-n, C-f, C-b, M-b, M-f, C-a, C-e, M-a, M-e, M-<, M->
* Edit
  <DEL>, C-d, M-<DEL>, M-d, C-k, M-k, C-/, C--, C-x u, C-g undo
* File&Buffer
  C-x C-f, C-x C-s, C-x s, C-x C-b, C-x b,	C-x C-c，C-x 1，C-x u
* Window
  C-x o, C-x 1, C-x 2, C-x 3


## Packages
### [Company](http://company-mode.github.io/)
```lisp
(add-hook 'after-init-hook 'global-company-mode)
```
* list enabled back-ends, `M-x customize-variable RET company-backends`
* more information, `M-x describe-function RET company-mode`
* customize other aspects of its behavior, `M-x customize-group RET company`

## Configuration For JavaScript&TypeScript
* Company

* [TypeScript in Emacs](http://redgreenrepeat.com/2018/05/04/typescript-in-emacs/)
## Configuration For C++
* Company
* Irony
* RTags

* [Emacs as a C++ IDE](http://martinsosic.com/development/emacs/2017/12/09/emacs-cpp-ide.html)
* [Using Emacs as a C++ IDE](https://oremacs.com/2017/03/28/emacs-cpp-ide/)
* [C/C++ Development Environment for Emacs](https://tuhdo.github.io/c-ide.html)

## Configuration For C#

* [C# autocompletion in Emacs](https://bbbscarter.wordpress.com/2013/08/17/c-autocompletion-in-emacs/)

# Configuration For Go

* [Writing Go in Emacs](http://dominik.honnef.co/posts/2013/03/writing_go_in_emacs/)


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
   
