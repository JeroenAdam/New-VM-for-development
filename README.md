### New virtual machine for development

I'm a fan of Ghost Spectre's Windows 11 Superlite, a trimmed-down version of the original Windows. We'll be using it in our new virtual machine. You can download it [here](https://ghostclouds.xyz/wp/w11-24h2-pro).

#### Prerequisites
Ensure Hyper-V is enabled on your system.

#### Setting Up the Virtual Machine

1. **Open PowerShell as an Administrator** and execute the following commands to set up your new VM:

    ```powershell
    New-VMSwitch -SwitchName NATSwitch -SwitchType Internal  
    New-NetIPAddress -IPAddress 10.0.0.1 -PrefixLength 24 -InterfaceAlias "vEthernet (NATSwitch)"
    New-NetNAT -Name NATNetwork -InternalIPInterfaceAddressPrefix 10.0.0.0/24
    New-VHD -Path "D:\DEV-W11.vhdx" -SizeBytes 40GB -Dynamic
    New-VM -Name DEV-W11 -MemoryStartupBytes 6GB -VHDPath "D:\DEV-W11.vhdx" -BootDevice VHD -Generation 2 -Switch NATSwitch
    Set-VMProcessor -VMName DEV-W11 -Count 4 -ExposeVirtualizationExtensions $true
    Add-VMDvdDrive -VMName DEV-W11 -Path "D:\iso\WIN11.PRO.24H2.X64.(WPE).ISO"
    $dvd = Get-VMDvdDrive -VMName DEV-W11
    Set-VMFirmware -VMName DEV-W11 -FirstBootDevice $dvd -EnableSecureBoot Off 
    ```

2. **Launch Hyper-V Manager**, start the VM and connect to it. Quickly tap a keyboard key to boot from DVD.

3. **Installation**:
    - Choose Windows 11 Pro Superlite (without Defender) during the setup process.
    - After reaching the desktop, some scripts will execute automatically, a reboot will follow.

4. **Configure Network Settings**:
    - Change the network adapter settings as follows:
        - **IP Address**: 10.0.0.2
        - **Subnet Mask**: 255.255.255.0
        - **Gateway**: 10.0.0.1
        - **DNS**: 8.8.8.8

5. **Install a Browser**:
    - This OS comes without a pre-installed browser. Launch 'Ghost toolbox' from the desktop and choose option 51.
    - I chose Brave as my browser.

6. **Install Chocolatey**:
    - Follow the instructions on the [Chocolatey installation page](https://chocolatey.org/install).
    - Relaunch PowerShell as an Administrator and install your favorite software using `choco install <software-name>`.

That's it! You now have a new VM set up and ready for your development purposes.
