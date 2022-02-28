# Git

https://git-scm.com/

    git config --global user.name "name"
    git config --global user.email "email"

    git config --global alias.st status
    git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

---

# Node.js

https://nodejs.org/ko/ (Windows)

https://github.com/nodesource/distributions (Linux)

---

# VSC

https://code.visualstudio.com/ (program)

or

https://github.com/cdr/code-server (web-based)

## Prettier

1. Settings > search `foramt on save` and check it
2. Click `Select Language Mode`
3. Click `Configure '~' language based settings...`
4. Add `"editor.defaultFormatter": "esbenp.prettier-vscode"`

example

    {
        ...
        "[javascript]": {
            "editor.defaultFormatter": "esbenp.prettier-vscode"
        },
    }

---

# Terminal

Settings > Terminal > integrated > Shell : Windows

Click `Edit in settings.json`

    "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe",

Change the contents.

---

# VSC extentions

1. Auto Close Tag

   > Id: formulahendry.auto-close-tag
   >
   > Description: Automatically add HTML/XML close tag, same as Visual Studio IDE or Sublime Text
   >
   > Publisher: Jun Han

2. Auto Rename Tag

   > Id: formulahendry.auto-rename-tag
   >
   > Description: Auto rename paired HTML/XML tag
   >
   > Publisher: Jun Han

3. TSLint

   > Id: ms-vscode.vscode-typescript-tslint-plugin
   >
   > Description: TSLint support for Visual Studio Code
   >
   > Publisher: Microsoft

4. vscode-icons

   > Id: vscode-icons-team.vscode-icons
   >
   > Description: Icons for Visual Studio Code
   >
   > Publisher: VSCode Icons Team

5. vscode-styled-components

   > Id: jpoissonnier.vscode-styled-components
   >
   > Description: Syntax highlighting for styled-components
   >
   > Publisher: Julien Poissonnier

6. Better Comments

   > Id: aaron-bond.better-comments
   >
   > Description: Improve your code commenting by annotating with alert, informational, TODOs, and more!
   >
   > Publisher: Aaron Bond

7. Bracket Pair Colorizer

   > Id: coenraads.bracket-pair-colorizer
   >
   > Description: A customizable extension for colorizing matching brackets
   >
   > Publisher: CoenraadS

8. Color Highlight

   > Id: naumovs.color-highlight
   >
   > Description: Highlight web colors in your editor
   >
   > Publisher: Sergii Naumov

9. IntelliSense for CSS class names in HTML

   > Id: zignd.html-css-class-completion
   >
   > Description: CSS class name completion for the HTML class attribute based on the definitions found in your workspace.
   >
   > Publisher: Zignd

10. Markdown All in One

    > Id: yzhang.markdown-all-in-one
    >
    > Description: All you need to write Markdown (keyboard shortcuts, table of contents, auto preview and more)
    >
    > Publisher: Yu Zhang

11. markdownlint

    > Id: davidanson.vscode-markdownlint
    >
    > Description: Markdown linting and style checking for Visual Studio Code
    >
    > Publisher: David Anson

12. Live Server
    > Id: ritwickdey.liveserver
    >
    > Description: Launch a development local Server with live reload feature for static & dynamic pages
    >
    > Publisher: Ritwick Dey
