# clink
コマンドプロンプトでセッション越しの履歴共有、なんちゃって補完

## install
```cmd
winget install clink
```

## setup
```cmd
clink set history.shared true
reg add "HKCU\Software\Microsoft\Command Processor" /v AutoRun /t REG_SZ /d "C:\Program Files (x86)\clink\clink_x64.exe" inject
```

## update
```cmd
rem 管理者権限が必要
rem run as administrator
clink update
```

## Windows Terminal
設定 > プロファイル > コマンドライン
```
cmd /s /k clink inject
```
