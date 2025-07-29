# Optimizacion
# optimizacion hdd 
---
### Desactiva el servicio Superfetch (SysMain)
---
### En la misma ventana services.msc, busca SysMain
---
# CMD ADMINISTRADOR 
---
### schtasks /Create /TN "Desfragmentar disco" /TR "defrag C: /O" /SC DAILY /ST 02:00 /RL HIGHEST
---
# Powershell ADMIN
---
### Add-MpPreference -ExclusionPath "C:\Program Files\Windows Defender"
---
### Add-MpPreference -ExclusionProcess "MsMpEng.exe"
---
### sc stop WinDefend
---
# Para ram 
---
### Remove-Item "$env:TEMP\*" -Recurse -Force -ErrorAction SilentlyContinue; [System.GC]::Collect(); [System.GC]::WaitForPendingFinalizers(); [System.GC]::Collect()
---
# repita cada 2 horas
---
### Guarda este comando en un script: C:\Scripts\limpiar.ps1 
---
### ExecutionPolicy Bypass -File "C:\Scripts\limpiar.ps1"
