# GalaxyBook 4 Edge - Fresh Windows reinstall
How to reinstall Windows 11 (ARM) on the GalaxyBook 4 Edge.

## Long Story Short:
I screwed up my laptop by doing a clean installation of Windows as if it were an x86 machine, and let me tell you, I was really "on the edge".
But thanks to me messing everything up, you get to enjoy this tutorial right now 👌

## You’ll need:
- A copy of the drivers, or you can download [mine](https://1024terabox.com/s/1cYVHED2nhbd3S3cM1zGlaQ)
- [CreatePartitions Script](https://1024terabox.com/s/1GL2oZdSyj4WEn1o7h5S55Q) and [ApplyImage script](https://1024terabox.com/s/1SDr4T6cQp9GyqAXK44GOqg).
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
Or download my copy of the drivers at this link: [Galaxy Book 4 Edge Drivers](https://1024terabox.com/s/1cYVHED2nhbd3S3cM1zGlaQ)

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

## 2. Mount the Windows ARM ISO and Add Drivers

1. **Download WinARM ISO**:
   - Download the ISO from the [official site](https://www.microsoft.com/en-gb/software-download/windows11arm64?msockid=19c1f599bc176a070fb9e01dbd166bd1) of Microsoft.
     
2. **Mount the ISO**:
   - Right-click the downloaded Windows ARM ISO file.
   - Select "Mount" to mount the ISO as a virtual drive.

3. **Locate the Install.wim File**:
   - Open the mounted ISO drive in File Explorer.
   - Navigate to the `sources` folder.
   - Copy the `install.wim` file to a convenient location on your PC, for example, `C:\wim`.

4. **Open Command Prompt as Administrator**:
   - Right-click on the Windows icon and select "Command Prompt (Admin)".

5. **Create a Mount Directory**:
   - Create a directory to mount the WIM file:
     ```cmd
     mkdir C:\wimmount
     ```

6. **Mount the Install.wim File**:
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
   - The WIM file should now be available at `C:\wim` or in the directory where you copied the WIM file.

## 3. Burn the second USB

1. **Download the scripts**:
   - Download the [CreatePartitions Script](https://1024terabox.com/s/1GL2oZdSyj4WEn1o7h5S55Q) and the [ApplyImage script](https://1024terabox.com/s/1SDr4T6cQp9GyqAXK44GOqg).
  
2. **Format the second USB**:
   - Format the USB drive as NTFS.
  
3. **Copy the files**:
   - Copy your WIM file and both scripts: CreatePartitions.txt & ApplyImage.bat to the second USB.
  
## 4. Prepare your Galaxy Book 4 Edge

1. **Plug both USB**:
   - With the PC off, plug your USB HUB with both USB drives.

2. **Enter to the BIOS**:
   - Turn on your computer by repeatedly pressing F2.
  
3. **Turn off secure boot**:
   - Turn off secure boot and fast boot.
  
4. **Change the boot order**:
   - Find the boot order and change it to boot the first (WinPE) USB.

5. **Save changes**:
   - Almost there...

## 5. Run the scripts
1. Wait until the command prompt window on a plain blue background appears.
2. Run `dikspart`.
3. Run `list volume` to identify the drive letter for the disk containing the install.wim and script files.
4. Run `exit` to go back.
5. Run the following commands replacing D: with the drive letter found with `list volume`:
```cmd
diskpart /s D:\CreatePartitions.txt
```
```cmd
D:\ApplyImage.bat D:\install.wim
```
If everything worked well, run exit and disconnect the USB drives before the system restarts.

## Thats it! 🙌
From this point onward, the installation process of Windows ARM 11 should begin, with all drivers included 🫡



