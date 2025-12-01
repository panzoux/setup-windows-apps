# yazi

## 事前インストール
* git (for windows)
* 7-zip
* vim
    * https://github.com/vim/vim-win32-installer/releases
    * wingetだとコマンドへのパスが手動になる。インストーラなら選択できる
* 白源フォントNF
    * https://github.com/yuru7/HackGen/releases/download/v2.10.0/HackGen_NF_v2.10.0.zip
    * *-NF.ttf を使うこと！ (Nerd-Fontsに含まれるアイコン文字でフォルダやファイルのアイコン的な表示をしている)
    * powershellでcopy-itemしても、ショートカットが作成されたため、手動で
* yaziが使うソフトウェア
    ```ps1
    # winget install xx yy zz でよいらしい…
    @("7zip.7zip","Gyan.FFmpeg","jqlang.jq","oschwartz10612.Poppler","sharkdp.fd","BurntSushi.ripgrep.GNU","junegunn.fzf","ajeetdsouza.zoxide","mpv","MediaArea.MediaInfo")|%{winget install $_}
    ```


## wt.exeの コマンドプロンプト(とか)の外観でフォント変更

```cmd
winget install sxyazi.yazi
```

ここに配置される
```
C:\Users\<user>\AppData\Local\Microsoft\WinGet\Packages\sxyazi.yazi_Microsoft.Winget.Source_8wekyb3d8bbwe\yazi-x86_64-pc-windows-msvc\yazi.exe
```

## 環境変数設定
git for windowsについてくるfile.exeのパスを環境変数 YAZI_FILE_ONE に入れる

```ps1
# file.exe のパス取得
$path_to_file=(join-path (split-path (split-path (where.exe git) -parent) -Parent) 'usr\bin\file.exe')

<#
# システム環境変数に設定
[Environment]::SetEnvironmentVariable("YAZI_FILE_ONE", $lpath_to_file, [EnvironmentVariableTarget]::Machine)
[Environment]::GetEnvironmentVariable("YAZI_FILE_ONE", "Machine")
#>

# ユーザ環境変数に設定
[Environment]::SetEnvironmentVariable("YAZI_FILE_ONE", $path_to_file, [EnvironmentVariableTarget]::User)
[Environment]::GetEnvironmentVariable("YAZI_FILE_ONE", "User")
```

## 設定ファイル準備
```ps1
# 空の設定ファイル作成
$confdir = "$env:AppData\yazi\config\"
@("yazi.toml","keymap.toml","theme.toml")|%{new-item (join-path $confdir $_)}

# presetファイル配置
pushd $env:temp
git clone https://github.com/sxyazi/yazi.git
robocopy "$env:temp\yazi\yazi-config\preset\" $confdir *
robocopy /e "$env:temp\yazi\yazi-plugin\preset\" $confdir
popd
```

## yコマンドの設定

パスが通った場所へ配置

**"y.bat"** コマンドプロンプト用
```bat
@echo off

set tmpfile=%TEMP%\yazi-cwd.%random%

yazi %* --cwd-file="%tmpfile%"

:: If the file does not exist, then exit
if not exist "%tmpfile%" exit /b 0

:: If the file exist, then read the content and change the directory
set /p cwd=<"%tmpfile%"
if not "%cwd%"=="" (
    cd /d "%cwd%"
)
del "%tmpfile%"
```


**$profile** powershell用
```ps1
if (!(Test-Path -Path $PROFILE)) {New-Item -ItemType File -Path $PROFILE -Force}
$a=@'
function y {
    $tmp = (New-TemporaryFile).FullName
    yazi $args --cwd-file="$tmp"
    $cwd = Get-Content -Path $tmp -Encoding UTF8
    if (-not [String]::IsNullOrEmpty($cwd) -and $cwd -ne $PWD.Path) {
        Set-Location -LiteralPath (Resolve-Path -LiteralPath $cwd).Path
    }
    Remove-Item -Path $tmp
}
'@
$a| out-file -append $profile
```


**バージョンによってパスが異なる**
|app|$profile path|
|-|-|
|powershell.exe (5.1)|C:\Users\user\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1|
|Powershell ISE|C:\Users\<user>\Documents\WindowsPowerShell\Microsoft.PowerShellISE_profile.ps1|
|pswh.exe (7.5)|C:\Users\user\Documents\PowerShell\Microsoft.PowerShell_profile.ps1|


# border
```cmd
ya pkg add yazi-rs/plugins:full-border
```

**init.lua**
```init.lua
require("full-border"):setup {
	-- Available values: ui.Border.PLAIN, ui.Border.ROUNDED
	-- type = ui.Border.PLAIN
	type = ui.Border.ROUNDED,
}
```
