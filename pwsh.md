# powershell 7+

## install

```ps1
winget install Microsoft.Powershell 
```

## setup

* psreadline
```ps1
$a=@"
Set-PSReadLineOption -WordDelimiters ';:,.[]{}()/\|^&*-=+''''`" !?@#$%&_<>「」（）『』『』［］、，。：；／'
Set-PSReadlineOption -HistoryNoDuplicates
Set-PSReadLineOption -HistorySavePath $env:userprofile\.powershell_history
"@
$a| out-file -append $profile
```
