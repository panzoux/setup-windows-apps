# alacritty
https://alacritty.org/

# install
https://github.com/alacritty/alacritty/releases/download/v0.16.1/Alacritty-v0.16.1-installer.msi

# setup

%APPDATA%\alacritty\alacritty.toml

```ps1
# reset config file
$configfile = "$env:AppData\alacritty\alacritty.toml"
ri $configfile

# create config file
$configfile = "$env:AppData\alacritty\alacritty.toml"
if (!(Test-Path -Path $configfile)) {New-Item -ItemType File -Path $configfile -Force}
$a=@'
[font]
normal = { family = "HackGen Console NF", style = "Regular"}
bold = { family = "HackGen Console NF", style = "Bold"}
'@
$a| out-file -append $configfile -encoding UTF8
```

## usage

ctrl+shift+space : vi-mode (i to go back to prompt)
