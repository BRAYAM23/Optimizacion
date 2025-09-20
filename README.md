
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
- while ($true) {
-    $processes = Get-Process | Where-Object { $_.WorkingSet64 -gt 100MB }
-   foreach ($p in $processes) {
-        try {
-            $handle = $p.Handle
-            [PsAPI]::EmptyWorkingSet($handle) | Out-Null
-            Write-Output "$(Get-Date): Liberado: $($p.ProcessName)"
-        } catch {
-            Write-Output "$(Get-Date): No se pudo liberar: $($p.ProcessName)"
-        }
-    }
-
-    Start-Sleep -Seconds 7200  # Esperar 2 horas
- }

# Powershell ADMIN (Solo cuando se tenga otro antiviruz)
- Add-MpPreference -ExclusionPath "C:\Program Files\Windows Defender"
- Add-MpPreference -ExclusionProcess "MsMpEng.exe"
- sc stop WinDefend

# mirar slots de la ram 
- wmic MemoryChip get BankLabel, DeviceLocator, Capacity

# mirar serial poweshell
- wmic memorychip get Manufacturer, PartNumber, SerialNumber, Capacity

- (Opcional) Instalar PHP
sudo apt install php libapache2-mod-php -y
sudo systemctl restart apache2
# comprobar
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
# luego visitar http://IP_DEL_SERVIDOR/info.php

Crear sitio web mínimo
sudo mkdir -p /var/www/misitio
sudo chown -R $USER:$USER /var/www/misitio
sudo chmod -R 755 /var/www/misitio
echo "<h1>Servidor Apache en Ubuntu</h1>" | sudo tee /var/www/misitio/index.html

VirtualHost (archivo de configuración del sitio)

Crea /etc/apache2/sites-available/misitio.conf con:

<VirtualHost *:80>
    ServerAdmin admin@misitio.com
    DocumentRoot /var/www/misitio
    ServerName misitio.com
    ServerAlias www.misitio.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>


Habilitar sitio y reiniciar:

sudo a2ensite misitio.conf
sudo systemctl restart apache2
# si quieres deshabilitar el default:
sudo a2dissite 000-default.conf
sudo systemctl reload apache2

Habilitar mod_rewrite (si hace falta)
sudo a2enmod rewrite
sudo systemctl restart apache2

4) Firewall (UFW) — permitir Apache

Verifica estado:

sudo ufw status verbose


Si UFW está activo, permite Apache:

sudo ufw allow 'Apache Full'   # abre 80 y 443
sudo ufw reload
sudo ufw status
