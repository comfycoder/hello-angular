[Home](README.md)

# Developer Tools Setup

Download and install the following software on your workstation.

# Notepad ++
https://notepad-plus-plus.org/downloads/

Run the install using most of the default selections.


# Visual Studio Code (great IDE for JavaScript) 
https://code.visualstudio.com/?wt.mc_id=vscom_downloads 

Run the install using most of the default selections. Allow it to add commands to Windows Explorer.

## VS Code Extensions

### Download and install the following extensions for VS Code:

[Angular Language Service](https://marketplace.visualstudio.com/items?itemName=Angular.ng-template)

[TSLint](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-tslint-plugin)

[vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons)

### Other highly recommended extensions

[Azure Repos](https://marketplace.visualstudio.com/items?itemName=ms-vsts.team)

[Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)

[Prettier - Code Formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

[Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)

[Live Share](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare)

[Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)

[Paste JSON as Code](https://marketplace.visualstudio.com/items?itemName=quicktype.quicktype)


# Git 
https://git-scm.com/download/win

Run the install using most of the default selections.

**NOTE**: Recommend choosing Notepad++ or VS Code as your default editor.
 

# NodeJS 
https://nodejs.org/en/ 

**NOTE**: Download the **LTS** version.

Open a command-line window and run the following commands:

```
npm install -g minimatch@latest 

npm install -g graceful-fs@latest 

npm install -g rimraf@latest 

npm install -g typescript@latest 

npm install -g tslint@latest 
```


# Yarn 
https://yarnpkg.com/en/ 

Open a command-line window and run the following commands:

```
yarn config set cache-folder c:\yarn 

yarn global add @angular/cli@latest 

ng config -g cli.packageManager yarn
```

[Home](README.md)

[Create A New Angular SPA](Create-A-New-Angular-SPA.md)