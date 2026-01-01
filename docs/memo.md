# PC設定

## ソフトウェア

- Chrome
- VSCode
- Rancher Desktop
- Terminal
- WSL2
- keypass
- chatGPT

## wsl 設定

### 基本設定

```bash
# sudo時のパスワード省略
$ sudo vi /etc/sudoers
${ユーザー名} ALL=NOPASSWD: ALL
```

### ssh

```bash
# 鍵生成
$ ssh-keygen

# ssh-agent用設定
## socat
$ sudo apt install socat -y

## npiperelay.exe
$ wget https://github.com/jstarks/npiperelay/releases/download/v0.1.0/npiperelay_windows_amd64.zip
$ unzip npiperelay_windows_amd64.zip
$ rm -f LICENSE README.md npiperelay_windows_amd64.zip
$ mv npiperelay.exe /mnt/c/Users/${ユーザー名}/

$ vi ~/.bashrc
export SSH_AUTH_SOCK=$HOME/.ssh/agent.sock
ss -a | grep -q $SSH_AUTH_SOCK
if [ $? -ne 0 ]; then
  rm -f "$SSH_AUTH_SOCK"
  (setsid socat UNIX-LISTEN:$SSH_AUTH_SOCK,fork EXEC:"/mnt/c/Users/${ユーザー名}/npiperelay.exe -ei -s //./pipe/openssh-ssh-agent",nofork &) >/dev/null 2>&1
fi
```

### Git

```bash
$ git config --global user.name "borikatsu"
$ git config --global user.email "xxxx"
```

### java

```bash
$ sudo apt install openjdk-25-jdk -y

$ vi ~/.bash_profile
# java
export JAVA_HOME=/usr/lib/jvm/java-25-openjdk-amd64
```

### node

<https://github.com/nvm-sh/nvm>

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# インストール可能バージョン一覧
$ nvm ls-remote

$ nvm install v24.12.0

# yarn
$ npm install -g yarn
```

## Windows 設定

### Terminal

- 既定のプロファイルをUbuntuに変更
- 「操作」選択範囲をクリップボードに自動でコピー

### OpenSSH

```shell
# インストール
$cap = Get-WindowsCapability -Online | ? Name -like 'OpenSSH.Client*'
Add-WindowsCapability -Online -Name $cap.Name

# 自動起動
Set-Service -Name ssh-agent -StartupType Automatic
Start-Service -Name ssh-agent
```

### keypassXC

<https://keepassxc.org/download/#windows>

1. データベース作成
1. エントリー設定
    1. ユーザー名：git
    1. パスワード：適宜
1. エントリー→「詳細設定」の添付ファイルに秘密鍵を設定
1. エントリー→「SSHエージェント」の添付ファイルに秘密鍵を設定
1. アプリケーション設定→「SSHエージェント」のOpebSSHを使用するにチェック

### Rancher Desktop

<https://rancherdesktop.io/>
