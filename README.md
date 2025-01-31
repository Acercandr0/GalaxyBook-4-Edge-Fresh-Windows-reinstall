# GalaxyBook 4 Edge - Fresh Windows reinstall
How to reinstall Windows 11 (ARM) on the GalaxyBook 4 Edge.

## Long Story Short:
I screwed up my laptop by doing a clean installation of Windows as if it were an x86 machine, and let me tell you, I was really "on the edge" (🥁) of my seat the whole time. But thanks to me messing everything up, you get to enjoy this tutorial right now 👌

## You’ll need:
- A copy of the drivers, or you can download [mine](https://1024terabox.com/s/1ZrLJrO0Ii5WbNNMDnphrEg)
- Download the WinPE ISO
- 1 USB HUB (or two flash drives with USB-C ports)
- 2 USB Flash drives (16GB or more)
- A working PC with Windows, or a virtual machine on macOS/Linux

## How to backup Drivers

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

## 1. Creating a WinPE Boot Device

1. **Install Required Tools**:
   - From your Windows device, install the Windows Assessment and Deployment Kit (Windows ADK) and the Windows PE add-on. Follow the [official guide](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install] for installation instructions.

2. **Launch Deployment and Imaging Tools Environment**:
   - Open the "Deployment and Imaging Tools Environment" app with administrator privileges.

3. **Execute Commands**:
   - Run the following commands in the command prompt:
     ```cmd
     cd "C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\arm64"
     ```
     ```cmd
      copype arm64 C:\WinPE_arm64
     ```
     ```cmd
     Dism /Mount-Image /ImageFile:"en-us\winpe.wim" /index:1 /MountDir:"C:\WinPE_arm64\mount"
      ```
      ```cmd
     MakeWinPEMedia /ISO C:\WinPE_arm64 C:\WinPE_arm64.iso
      ```
     ```cmd
     Dism /Unmount-Image /MountDir:"C:\WinPE_arm64\mount" /commit
     ```

4. **Locate the WinPE ISO**:
   - The WinPE ISO should now be available at `C:\WinPE_arm64.iso`.

5. **Burn the first USB**
   - You can burn it with [Rufus](https://rufus.ie/en/).

## 2. Download a Windows ARM 11 copy
   - Download the ISO from the [official site](https://www.microsoft.com/en-gb/software-download/windows11arm64?msockid=19c1f599bc176a070fb9e01dbd166bd1) of Microsoft.

## 3. Mount the Windows ARM ISO and Add Drivers

1. **Mount the ISO**:
   - Right-click the downloaded Windows ARM ISO file.
   - Select "Mount" to mount the ISO as a virtual drive.

2. **Locate the Install.wim File**:
   - Open the mounted ISO drive in File Explorer.
   - Navigate to the `sources` folder.
   - Copy the `install.wim` file to a convenient location on your PC, for example, `C:\wim`.

3. **Open Command Prompt as Administrator**:
   - Right-click on the Windows icon and select "Command Prompt (Admin)".

4. **Create a Mount Directory**:
   - Create a directory to mount the WIM file:
     ```cmd
     mkdir C:\wimmount
     ```

5. **Mount the Install.wim File**:
   - Use the `dism` command to mount the WIM file:
     ```cmd
     dism /mount-wim /wimfile:C:\wim\install.wim /index:1 /mountdir:C:\wimmount
     ```

6. **Add Drivers to the Mounted WIM File**:
   - Copy your drivers backup to the `C:\Drivers` folder.
   - Use the `dism` command to add the drivers from `C:\Drivers`:
     ```cmd
     dism /image:C:\wimmount /add-driver /driver:C:\Drivers /recurse
     ```

7. **Unmount and Commit Changes**:
   - Unmount the WIM file and commit the changes:
     ```cmd
     dism /unmount-wim /mountdir:C:\wimmount /commit
     ```
     
8. **Locate the modified WIM file**:
   - The WIN file should now be available at `C:\wim`.





