
# OPTIMIZACION HDD 
### Desactiva el servicio Superfetch (SysMain)
- En la misma ventana services.msc, se busca SysMain
# CMD Administrador. 
### Este comando programa la desfragmentacion del disco a las 2 AM
- schtasks /Create /TN "Desfragmentar disco" /TR "defrag C: /O" /SC DAILY /ST 02:00 /RL HIGHEST
### Este comando elimina todos los archivos del directorio temporal del usuario
- del /q /f /s %TEMP%\*
### limpiar otras carpetas temporales:
- del /q /f /s C:\Windows\Temp\*
# Comandos de optimización de rendimiento general (CMD ADMIN)
### Desactiva hibernación y libera espacio en disco.
- powercfg -h off
### Estabiliza el reloj del sistema, mejora latencia en algunos casos.
- bcdedit /set useplatformclock true
### Desactiva ticks dinámicos, mejora estabilidad y latencia.
- bcdedit /set disabledynamictick yes
### Fuerza el uso del reloj de hardware, útil para gaming/latencia.
- bcdedit /set useplatformtick yes
### Configura limpieza profunda del disco.
- cleanmgr /sageset:1
### Ejecuta limpieza configurada arriba.
- cleanmgr /sagerun:1
### Escanea y repara archivos del sistema corruptos.
- sfc /scannow
### Repara imagen de Windows sin reinstalar.
- dism /online /cleanup-image /restorehealth
### Repara errores en el disco duro.
- chkdsk /f /r
### Limpia papelera de reciclaje a nivel de sistema.
- rd /s /q C:$Recycle.bin
### Borra caché DNS para mejorar navegación/red.
- ipconfig /flushdns
### Restaura el stack de red, útil para problemas de conexión.
- netsh winsock reset
# PowerShell como Administrador
### Desactiva efectos visuales (animaciones)
- Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects" -Name "VisualFXSetting" -Value 2
### desactivar efectos animaciones de las ventanas:
- Set-ItemProperty -Path "HKCU:\Control Panel\Desktop" -Name "UserPreferencesMask" -Value ([byte[]](0x90,0x12,0x03,0x80,0x10,0x00,0x00,0x00))
### Quitar efectos de transparencia
- Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Themes\Personalize" -Name "EnableTransparency" -Value 0
### Aplica los cambios de inmediato:
- RUNDLL32.EXE user32.dll,UpdatePerUserSystemParameters
### Habilita el plan de energía de máximo rendimiento si aún no existe
- powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
- hay que seleccionarlo manualmente
## Para ram 
- Remove-Item "$env:TEMP\*" -Recurse -Force -ErrorAction SilentlyContinue; [System.GC]::Collect(); [System.GC]::WaitForPendingFinalizers(); [System.GC]::Collect()
## repita cada 2 horas
- Guarda este comando en un script: C:\Scripts\limpiar.ps1 
- ExecutionPolicy Bypass -File "C:\Scripts\limpiar.ps1"
# Powershell ADMIN (Solo cuando se tenga otro antiviruz)
- Add-MpPreference -ExclusionPath "C:\Program Files\Windows Defender"
- Add-MpPreference -ExclusionProcess "MsMpEng.exe"
- sc stop WinDefend

