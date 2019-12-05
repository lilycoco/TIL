# yarn

`yarn install` の際に以下のエラーが出た場合、node のバージョンを変えなければいけない。
> The engine "node" is incompatible with this module.  Expected version ">=4 <=9".


https://github.com/nvm-sh/nvm

`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash`

`export NVM_DIR="$HOME/.nvm"`

`[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm`
 
`nvm install 10.15.3[バージョン指定]`

`nvm alias default v10.15.3`


create `~/.bash_profile`

fill in the file as below 
```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

`yarn install`
