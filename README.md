#!PS
#timeout=1000000

$ErrorActionPreference = "Stop"

$Url = "https://raw.githubusercontent.com/Firrelagrace/Java-Jre-8-461/main/jre-8u461-windows-i586.exe"
$Out = Join-Path $env:TEMP "jre-8u461-windows-i586.exe"

Write-Output "Downloading Oracle JRE 8u461 (x86) from GitHub..."
Invoke-WebRequest -Uri $Url -OutFile $Out -UseBasicParsing

if (-not (Test-Path $Out)) {
  throw "Download failed: $Out"
}

Write-Output "Installing silently..."
$Args = "/s REBOOT=Suppress AUTO_UPDATE=0"
$proc = Start-Process -FilePath $Out -ArgumentList $Args -Wait -PassThru

if ($proc.ExitCode -ne 0) {
  throw "Java install failed. ExitCode=$($proc.ExitCode)"
}

Write-Output "Installed Oracle JRE 8u461 (x86) successfully."
exit 0
