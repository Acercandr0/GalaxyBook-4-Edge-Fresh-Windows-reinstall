# GalaxyBook 4 Edge - Fresh Windows reinstall
How to reinstall Windows 11 (ARM) on the GalaxyBook 4 Edge.

### Long Story Short:
I screwed up my laptop by doing a clean installation of Windows as if it were an x86 machine, but thanks to me messing everything up, you get to enjoy this tutorial right now.

### You’ll need:
- A copy of the drivers, or you can download mine.
- 1 USB HUB (or two flash drives with USB-C ports)
- 2 USB Flash drives (16GB or more)
- A working PC with Windows, or a virtual machine on macOS/Linux

### 1. Backup Drivers

1. **Open Command Prompt as Administrator**:

2. **Create a folder to store the drivers**:
   - In the Command Prompt window, type the following command to create a folder named "Drivers" on the C: drive:
     ```cmd
     mkdir C:\Drivers
     ```

3. **Export the drivers**:
   - Use the following command to export the drivers to the folder you created:
     ```cmd
     dism /online /export-driver /destination:C:\Drivers
     ```
Or download my copy of the drivers at this link: [Galaxy Book 4 Edge Drivers](https://1024terabox.com/s/1ZrLJrO0Ii5WbNNMDnphrEg)



