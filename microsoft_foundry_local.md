# Microsoft Founlry Local

## install

https://learn.microsoft.com/ja-jp/azure/ai-foundry/foundry-local/get-started?view=foundry-classic&source=recommendations

```ps1
winget install Microsoft.FoundryLocal
winget install OpenJS.NodeJS
```

## update

```ps1
winget upgrade --id Microsoft.FoundryLocal
```

## restart service

```ps1
foundry service restart
```

## command sample

```ps1
foundry model run qwen2.5-0.5b
foundry model run qwen2.5-coder-14b
foundry model run phi-4-mini
foundry model list
foundry cache list
foundry cache remove qwen2.5-coder-14b-instruct-generic-cpu:4
```


## sample project

### using python
* install python,uv
```ps1
cd $env:userprofile\source\temp\Foundry-Local\samples\python\summarize
uv init -p 3.13
ri main.py
pip install -r requirements.txt
python .\summarize.py --model qwen2.5-coder-14b aaa.txt
```


### using powershell

```ps1
foundry service start --verbose
foundry service stop
```

```
🟢 Service is already running on http://127.0.0.1:57349/.
```

```ps1
$url=((foundry service start) -replace '^.* |\.$','') + "v1/chat/completions"
$response = Invoke-RestMethod -Uri $url -Method Post -ContentType "application/json" -Body (@{
model = "Phi-4-mini-instruct-generic-cpu:5"
messages = @(@{ role = "user"; content = "how co you calculate pi?" })
} | ConvertTo-Json -Depth 3)
write-output $response.choices[0].message.content
```

```ps1
function ai {
    param ([string]$userinput)
    write-host "input: $userinput"
    if (-not [string]::IsNullOrWhiteSpace($userinput)){
        $url=((foundry service start) -replace '^.* |\.$','') + "v1/chat/completions"
        $response = Invoke-RestMethod -Uri $url -Method Post -ContentType "application/json" -Body (@{
        model = "Phi-4-mini-instruct-generic-cpu:5"
        messages = @(@{ role = "user"; content = $userinput })
        } | ConvertTo-Json -Depth 3)
        write-output $response.choices[0].message.content
    }
}
```

