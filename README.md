## This repository examples of basic PowerShell Scripts.

* Find large files in a particular directory.

```PowerShell
$path = Read-Host "Please enter the path you wish to scan for large files."

if (Test-Path $path) {
    $fileNames = Get-ChildItem -path $path -Recurse
    $largeFiles = $fileNames | Where-Object{$_.Length -gt 100MB}
    $largeFilesCount = $largeFiles | Measure-Object | Select-Object -ExpandProperty Count
    Write-Host "The number of large files on the system are :$largeFilesCount"
    Write-Hose "The files are as below \n"
    foreach($file in $largeFiles){
        Write-Host "$file"
    } 
}
else{
    Write-Host "Path does not exists on your system."
}
```

* Find large processes running on the system using Where-Object.

```PowerShell
$process = Get-Process
$largeProcess = $process | Where-Object{$_.WorkingSet64 -gt 100MB} | Select-Object Name, CPU, WorkingSet64
$largeProcessCount = $largeProcess.Length
Write-Host "The no of large processes currently running are $largeProcessCount"
Write-Host "Processes are:"
foreach ($proc in $largeProcess) {
    Write-Host "$proc"
}
```

* Web Request to Powershell Reddit to get top 10 posts.

```PowerShell
$webResult = Invoke-WebRequest -Uri 'https://reddit.com/r/powershell.json'
$rawData = $webResult.Content | ConvertFrom-Json
$posts = $rawData.data.children.data
$top = $posts | Select-Object Title,Score | Sort-Object Score -Descending | Select-Object -First 10   

foreach($post in $top)
{
    Write-Host "$post"
}
```

* API call

```PowerShell
[int]$num = Read-Host "Enter number of cats facts you would like:"
$catData = Invoke-RestMethod -Uri "https://catfact.ninja/facts?limit=$num&max_length=140"
$catData.data
```

* Create a Profile in PowerShell

```PowerShell
#We can define any function in the profile script which will run whenever we open powershell.
if(Test-Path $profile){
    Write-Host "Profile already exists"
}
else{
    Write-Host "Profile does not exist. Creating a new profile...."
    New-Item -Path $profile -Type file -force
}
```