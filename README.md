

# **Windows Cleanup Commands** 🧹🖥️

A collection of **CMD** and **PowerShell** commands to clean leftover data, orphaned files, and registry entries that remain after software uninstallation, along with other useful maintenance commands.

*Written by Kamrul Hossain*

---

## **Cleanup Commands** 🛠️

### 1. **Delete Temporary Files** 🗑️  
Remove unnecessary temporary files that can accumulate in the system.

- **CMD Command**:  
  ```bash
  del /q/f/s %TEMP%\*
  ```
  ```bash
  del /q/f/s C:\Windows\Temp\*
  ```

- **PowerShell Command**:  
  ```bash
  Remove-Item -Path $env:TEMP\* -Recurse -Force
  ```
  ```bash
  Remove-Item -Path "C:\Windows\Temp\*" -Recurse -Force
  ```

---

### 2. **Clear Prefetch Files** ⚡  
Clears cached data from application launches that could cause system slowdowns.

- **CMD Command**:  
  ```bash
  del /q/f/s C:\Windows\Prefetch\*
  ```

- **PowerShell Command**:  
  ```bash
  Remove-Item -Path "C:\Windows\Prefetch\*" -Recurse -Force
  ```

---

### 3. **Flush DNS Cache** 🌐  
Clear the DNS cache to avoid outdated network data.

- **CMD Command**:  
  ```bash
  ipconfig /flushdns
  ```

---

### 4. **Clear Software Distribution Folder** 📥  
Remove Windows update files and cached data that are no longer needed.

- **CMD Command**:  
  ```bash
  net stop wuauserv
  ```
  ```bash
  del /q/f/s %windir%\SoftwareDistribution\*
  ```
  ```bash
  net start wuauserv
  ```

---

### 5. **Remove Orphaned Registry Entries** 🔧  
Remove leftover registry entries of uninstalled software.

- **PowerShell Command**:  
  ```bash
  $paths = @(
      "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall",
      "HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall"
  )

  foreach ($path in $paths) {
      Get-ChildItem -Path $path | ForEach-Object {
          if (!(Get-ItemProperty -Path $_.PSPath -Name DisplayName -ErrorAction SilentlyContinue)) {
              Remove-Item -Path $_.PSPath -Recurse -Force -ErrorAction SilentlyContinue
          }
      }
  }
  Write-Host "Unused uninstall entries removed."
  ```

---

### 6. **Clean Up Windows Update Cache** 🔄  
Delete old update cache files that can take up space.

- **CMD Command**:  
  ```bash
  net stop wuauserv
  ```
  ```bash
  del /q/f/s %windir%\SoftwareDistribution\Download\*
  ```
  ```bash
  net start wuauserv
  ```

---

### 7. **Delete Old Log Files** 📝  
Clear system log files that may have accumulated.

- **CMD Command**:  
  ```bash
  del /q/f/s C:\Windows\Logs\*
  ```

---

### 8. **Delete System Restore Points** 🔄  
Remove old system restore points to free up disk space.

- **CMD Command**:  
  ```bash
  vssadmin delete shadows /for=c: /all
  ```

**⚠️ Caution**: This command deletes all system restore points. Ensure you have a recent restore point before running it.

---

### 9. **Disable Hibernation** 🛏️  
Disabling hibernation will delete the **hiberfil.sys** file, which can be quite large and takes up disk space.

- **CMD Command**:  
  ```bash
  powercfg -h off
  ```

---

### 10. **Clean Windows Prefetch Cache** 📂  
The prefetch cache stores information for faster booting. Over time, it can become bloated.

- **CMD Command**:  
  ```bash
  del /q/f/s C:\Windows\Prefetch\*
  ```

---

### 11. **Remove Old Windows Installation Files** 🗄️  
If you recently upgraded Windows (e.g., from Windows 7/8 to Windows 10/11), old Windows installation files may still be occupying significant disk space.

- **CMD Command**:  
  ```bash
  del /q/f/s %SystemDrive%\$Windows.~BT
  ```
  ```bash
  del /q/f/s %SystemDrive%\$Windows.~WS
  ```
  ```bash
  del /q/f/s C:\Windows\Installer\*
  ```

---

### 12. **Remove Unused Device Drivers** 🖱️  
Over time, unused or old device drivers can accumulate in your system.

- **CMD Command**:  
  ```bash
  set devmgr_show_nonpresent_devices=1
  ```
  ```bash
  start devmgmt.msc
  ```

This command opens **Device Manager** with non-present devices visible. You can manually remove old, unused drivers from there.

---

### 13. **Clear Windows Event Logs** 📋  
Windows Event Logs can grow over time and take up disk space.

- **CMD Command**:  
  ```bash
  wevtutil cl System
  ```
  ```bash
  wevtutil cl Application
  ```
  ```bash
  wevtutil cl Security
  ```

---

### 14. **Clean Windows Search Index** 🔍  
The Windows Search Index can accumulate unnecessary files over time.

- **CMD Command**:  
  ```bash
  del /q/f/s C:\ProgramData\Microsoft\Search\Data\Applications\Windows\*
  ```

---

## **Important Notes** ⚠️

- **Registry Cleanup**: Be cautious when manually deleting registry keys. Deleting the wrong entries can affect system stability. **Always back up your system or registry** before performing cleanup operations.
- **System Restore**: It’s generally a good idea to keep some system restore points, as they can be useful for troubleshooting in case something goes wrong.

For safer, more automated solutions, consider using third-party tools like **CCleaner** or **Revo Uninstaller**.
