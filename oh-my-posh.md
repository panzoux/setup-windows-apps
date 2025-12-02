# oh-my-posh

## prerequisistes

* clink (for cmd)
* font: hackgen-nf

## install

```ps1
winget install JanDeDobbeleer.OhMyPosh --source winget
```

## setup

### cmd

1. clink

```ps1
$a=@"
load(io.popen('oh-my-posh init cmd'):read("*a"))()
"@
$a | out-file $env:LOCALAPPDATA\clink\oh-my-posh.lua
```

### powershell
```ps1
if (!(test-path $profile)){New-Item -Path $PROFILE -Type File -Force}
"oh-my-posh init pwsh | Invoke-Expression"| out-file -append $profile
```
