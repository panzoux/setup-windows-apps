# zoxide
powershellでパスの記録と移動

## install
```ps1
winget ajeetdsouza.zoxide
```

## zoxide用コマンド設定
```ps1
$a=@'
Invoke-Expression (& { (zoxide init powershell | Out-String) })
'@
$a| out-file -append $profile
```

どうも、最後のほうでないと動かない事例があるとのこと。yaziより後ろがよさそう。

$profileの再読み込み
```ps1
. $profile
```

これで、
* z でパスを保存
* zi で保存したパス選択、選択先へ移動
