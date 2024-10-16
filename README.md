# Lab Phase 4: Virtualization capability and installing VMs

## Research and Determine Ideal Virtualization Environment

Two common offerings in the virtualization marketspace are **VirtualBox** and **VMware**. Let’s make a quick comparison of the two platforms.

### VirtualBox
- Free and open source (Oracle)
- User-friendly UI, straightforward
- Limited snapshot functionality
- Superior extensions from community support (open source)

### VMware
- Free version available, as well as paid versions
- Generally superior performance
- Professional UI with more advanced features
- Advanced snapshots, including multiple snapshots and branching

VMware generally performs better, especially when running resource-intensive applications or multiple simultaneous VMs, which is one of the requirements of my virtual environment. It is more utilized in professional settings, which is another reason I should gain some experience using the platform. 

VirtualBox is highly regarded for its ease of use with beginners and casual users. It is a cost-effective choice for learners and people who prefer open-source options with a large amount of community support. It is seen positively for its ease of setup and simplicity.

I have personally used both and do not really have a strong preference. However, I have previous experience with Virtualbox and I would like to learn a new virtualizaion platform. Moving away from what you are comfortable with always presents an opportunity to learn something new. Although they are similar, some methods of configuration and available options may change slightly.

- [VirtualBox Official Website](https://www.virtualbox.org/) 
- [VMware Official Website](https://www.vmware.com/) 
- [VirtualBox Documentation](https://www.virtualbox.org/wiki/Documentation) 
- [VMware Documentation](https://docs.vmware.com/) 

## Download and Configure Virtualization Software

First, let’s go download a few pieces of software. This is a new PC in this scenario, so we are going to need some software to accomplish our tasks. Here is a short list of sites. Use reputable sources for software, preferably the developer’s hosted main download source. I take no responsibility for what you download from the internet.

- [VMware](https://www.vmware.com/) (This is kind of rough since Broadcom acquired VMware; the shift of where the download is located, and the process can be problematic. To be honest, the site design and UI are not friendly or easy to navigate)
- [7-zip](https://www.7-zip.org/) 
- [HWmonitor](https://www.cpuid.com/softwares/hwmonitor.html) (live system stats, like temps, utilization, voltages, and speeds for CPU, GPU, RAM, etc.)

Now, we need to make sure to enable some PC settings that will allow virtualization. This will be different for some users, depending on hardware, software, and especially which company manufactured your computer’s CPU. In my case that is AMD (not Intel). If you are not sure which manufacturer made your CPU, there are many ways to check. A quick and easy method is to do the following:

1. Right-click the Start (Windows) button on your taskbar.
2. Select **System**:
   - From the context menu, click on **System**.
3. View Processor Information:
   - In the System window, look under **Device specifications**.
   - Find the **Processor** entry to see if it's Intel or AMD.

Moving on, we are using an AMD processor, and I want to use the virtualization features of the AMD chip (called **AMD-V**). This will provide hardware-level support for our VMs, making them faster and more efficient than using the Windows software-based virtualization. So, we are going to make sure that AMD-V is enabled and that Windows Hyper-V is disabled.

We are going to need to enter our BIOS. If you have never done this, it may seem intimidating. Do not worry, you can change all kinds of settings, and it won’t matter as long as you don’t choose **Save & Exit**. If you are concerned, you can always just choose **Exit without Saving** and none of the changes will take place.

### Access BIOS/UEFI via Windows Settings
1. Click on the Start Menu and select **Settings** (gear icon).
2. Go to **Update & Security**.
3. Select the **Recovery** tab on the left.
4. Under the **Advanced startup** section, click **Restart now**.
5. After your PC restarts, select **Troubleshoot**.
6. Choose **Advanced options**.
7. Select **UEFI Firmware Settings** and then click **Restart**. This will take you to the BIOS/UEFI setup.

This will restart the PC and boot up to a very different menu.

### Enable these options in BIOS:
1. Look under the menu item **CPU configuration**
     - AMD-V (enable this)	
     - NX (No Execute)** (enable this)
2. Also, while we are in BIOS
     - Choose **power limit select**
     - Increase this from **15W (balanced)** to **35W (performance)**
     - Choose **UMA frame buffer size**
      - Change this from **1G** to **> Auto**
3. Now click **Save & Exit**
4. Allow the PC to reboot

This is where the HWmonitor program we downloaded earlier will come in handy. Using this lightweight program, we can monitor the PC statistics to make sure they remain in safe parameters (especially the temperatures, since we increased the power delivery to the system).

### Disable Hyper-V: 
1. Enter the **Windows Terminal**.
2. Type `bcdedit /set hypervisorlaunchtype off`.
3. Reboot the PC.
4. If we have problems later with VMware giving errors related to Hyper-V, come back to this section and try this command instead:
   - `bcdedit /set hypervisorlaunchtype auto`
   - **OR**
   - `DISM /Online /Disable-Feature: Microsoft-Hyper-V-All`
   - You would need to reboot after trying each one of these as well, meaning… 
     - try one command, reboot, and attempt virtualization; if an error is given,
     - try the other command, reboot, and attempt virtualization again.

At this point, we should be able to run a virtual environment in VMware because we enabled AMD-V (and NX) in BIOS and disabled Hyper-V in Windows.
1. Install VMware from your download.
2. Accept the EULA.
3. No license key should be required for the free version.

# Setting Up Virtual Machines in VMware

Now we can download some virtual machines to install in our new lab.

1. Go to [here to download a Kali VM](https://www.kali.org/get-kali/#kali-virtual-machines).
   - Find the VMware download link.
   - Click this link to download and unzip the file (7-zip).
2. Browse [here to download Metaspoitable](https://information.rapid7.com/metasploitable-download.html).
3. Or here at [SourceForge](https://sourceforge.net/projects/metasploitable/)
   - Downloading from sourceforge.net doesn’t require you to register.
4. Go [here](https://sourceforge.net/projects/owaspbwa/files/) to get OWASP on SourceForge.
5. Or [here](https://github.com/chuckfw/owaspbwa) to get it from GitHub instead. 
   - Download version 1.2 (last version 08/2015).
6. Create a folder wherever you like, such as `Documents > Virtual Machines`.
7. Create a folder for each machine inside the Virtual Machines folder, like so:
- Documents
   - Virtual Machines
      - Kali
      - Metaspoitable
      - OWASP

9. Move each unzipped machine file from downloads into its designated folder. Keep backups of the zipped versions for now in case you make a mistake and need to recreate the VM.

## Installing the VMs Inside the Virtualization Environment

There are many ways to perform this task. In this document, we will cover a simple method and discuss the configuration details.

1. Open the VMware application.
2. Click `File > Open` in VMware.
3. Point to the folder for the VM you are opening (e.g., Kali, OWASP, etc.).
4. Options for machine configuration will have default suggestions and max allowances. Different machines will have different requirements. You should research the internet for this information and assign resources to the VM accordingly. Take Kali for example:
1. **Memory (RAM)** – 2G suggested; I assign 4G for better performance.
2. **Processors** – 2 processors and 2 cores per processor (total 4).
3. **Hard Disk** – This is the amount of storage your VM will allocate for itself. I have chosen 80G. If you plan to have large files (wordlists for password cracking), you will need to assign enough storage space.

### Network Adapters Basics and What They Mean:

- **Bridged**: The VM connects directly to the physical network and gets an IP from the actual DHCP server on the network, not the virtual DHCP server inside VMware. The VM appears as an additional computer on the same physical network as the host system.
   - **DO NOT** run a vulnerable VM in bridged mode or expose one to the internet. It can be targeted by attackers.
- **NAT**: The VM shares the host IP and MAC of the host system (your PC) and is behind a virtual router. The VM can access the internet but is not accessible from outside the network. It is on a separate private network from the host and gets its IP from VMware’s DHCP server.
- **Host-only**: The VM is isolated and can only communicate with the host and other VMs on the same network. This creates a VPN between the host and VM that is not visible outside of the host system. This is the most secure option for running purposefully vulnerable machines.

There is much more detail to these network adapter settings and other possible network adapter settings as well. Please visit [this site](https://docs.vmware.com/en/VMware-Workstation-Player-for-Windows/17.0/com.vmware.player.win.using.doc/GUID-C82DCB68-2EFA-460A-A765-37225883337D.html) for the VMware documentation to read more detail. You may need to do additional research before you attempt to set up these machines.

We are using host-only adapters for the 3 machines we have previously mentioned. Except the Kali machine will also have an additional NAT network adapter in VMware so that it can access the internet to receive updates and packages. This internet access can be turned on/off in the VM so that it can be isolated for security and only connected to the internet when necessary.

1. **USB Interface**: This can be changed based on the type of USB ports and devices you may connect to the host that can also be used within the VM (2.0, 3.0, etc.).
2. **Sound Card**: Can be left as Auto Detect.
3. **Display**: Can be left as Auto Detect.

Clicking OK will now create the VM. We will not cover the internal environments of the VMs themselves in this section.

