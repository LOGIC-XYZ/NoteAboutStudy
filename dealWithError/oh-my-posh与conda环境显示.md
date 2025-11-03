时间              2025.11.03
起因：今天突然发现没有显示出当前的 `conda` 环境，于是又是折腾显示问题，包括一些别名、`~` 下不显示的一些问题。

### 显示问题
#### 原因分析：
在主题设置中 `python` 的相关配置中未设置显示 `Conda` 环境。
#### 解决路径：
在配置文件中的相关配置中设置：
```
  	  	    "fetch_virtual_env": true,
```

### 别名问题
#### 原因分析：
输入 `conda` 相关指令时报如下错误：
```shell
usage: conda-script.py [-h] [-v] [--no-plugins] [-V] COMMAND ... conda-script.py: error: argument COMMAND: invalid choice: '' (choose from 'activate', 'clean', 'commands', 'compare', 'config', 'create', 'deactivate', 'env', 'export', 'info', 'init', 'install', 'list', 'notices', 'package', 'build', 'content-trust', 'convert', 'debug', 'develop', 'doctor', 'index', 'inspect', 'metapackage', 'render', 'repoquery', 'skeleton', 'server', 'token', 'repo', 'pack', 'remove', 'uninstall', 'rename', 'run', 'search', 'update', 'upgrade')
```
且`Conda module` 每次重启终端时自动创建 `alias`，因为输入 `Get-Command conda` 报如下错误：
```shell
CommandType Name Version Source 
----------- ---- ------- ------ 
Alias conda -> Invoke-Conda 0.0 Conda
```
#### 解决路径：
1. 先在 `profile` 中加入以下两排以阻止 Conda module 自动创建 alias 
	```
	Remove-Module Conda -ErrorAction SilentlyContinue 
	Remove-Item alias:conda -ErrorAction SilentlyContinue 
	```
2. 然后在这两排下面加入如下语句
	```
	# ---- Conda Hook ---- 
	$condaProfile = "F:\Anaconda\shell\condabin\conda-hook.ps1" # 此处填入相应 ps1 的地址
	if (Test-Path $condaProfile) {
	    & $condaProfile | Out-Null
	    & "F:\Anaconda\Scripts\conda.exe" activate base
	} else {
	    Write-Host "Conda hook not found, run 'conda init powershell'" -ForegroundColor Yellow
	}
	```


#error 