# Preparing a Compatible Virtualization Environment

## Research and Determine Ideal Virtualization Environment

Two of the main offerings and competitors in the virtualization marketspace are **VirtualBox** and **VMware**. Let’s make a quick comparison of the two platforms.

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

I have personally used both and do not really have a strong preference. However, VirtualBox was the first VM software I learned to use. I already have experience with it, and I would like to learn how to use a different method of virtualization. Moving away from what you are comfortable with always presents an opportunity to learn something new. Although they are similar, methods of configuration and the available options will change slightly.

- [VirtualBox Official Website](https://www.virtualbox.org/) 
- [VMware Official Website](https://www.vmware.com/) 
- [VirtualBox Documentation](https://www.virtualbox.org/wiki/Documentation) 
- [VMware Documentation](https://docs.vmware.com/) 

## Download and Configure Virtualization Software

First, let’s go download a few pieces of software. This is a new PC in this scenario, so we are going to need some software to accomplish some tasks. Here is a short list of sites. Use reputable sources for software, preferably the developer’s hosted main download source. I take no responsibility for what you download from the internet.

- [VMware](https://www.vmware.com/) (This is kind of rough since Broadcom acquired VMware; the shift of where the download is located, and the process can be problematic. To be honest, the site design and UI are not friendly or easy to navigate)
- [7-zip](https://www.7-zip.org/) 
- [HWinfo](https://www.hwinfo.com/download/) (system information)
- [CPU-Z](https://www.cpuid.com/softwares/cpu-z.htm) (system information)
- [HWmonitor](https://www.cpuid.com/softwares/hwmonitor.html) (live system stats, like temps, utilization, voltages, and speeds for CPU, GPU, RAM, etc.)

Now, we need to make sure to enable some PC settings that will allow virtualization. This will be different for some users, depending on hardware, software, and especially which company manufactured your computer’s CPU. In my case that is AMD (not Intel). If you are not sure which manufacturer made your CPU, there are many ways to check. A quick and easy method is to do the following:

1. Right-click the Start (Windows) button on your taskbar.
2. Select **System**:
   - From the context menu, click on **System**.
3. View Processor Information:
   - In the System window, look under **Device specifications**.
   - Find the **Processor** entry to see if it's Intel or AMD.

Moving on, we are using an AMD processor, and I want to use the virtualization features of the AMD chip (called **AMD-V**). This will provide hardware-level support for our VMs, making them faster and more efficient than using the Windows software-based virtualization. So, we are going to make sure that AMD-V is enabled and that Windows Hyper-V is disabled.

We are going to need to enter our BIOS. If you have never done this, it may seem intimidating. Do not worry, you can change all kinds of settings, and it won’t matter as long as you don’t choose **Save & Exit**. If you get scared, you can always just choose **Exit without Saving** and none of the changes will take place.

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
- Look under the menu item **CPU configuration**
  - **AMD-V** (enable this)	
  - **NX (No Execute)** (enable this)
- Also, while we are in BIOS
  - Choose **power limit select**
  - Increase this from **15W (balanced)** to **35W (performance)**
  - Choose **UMA frame buffer size**
    - Change this from **1G** to **> Auto**
- Now click **Save & Exit**
- Allow the PC to reboot

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

We can check our settings with CPU-Z, which we downloaded earlier.
1. Install CPU-Z and run the program.
2. Look for **AMD-V** in green print; this means it is enabled.

At this point, we should be able to run a virtual environment in VMware because we enabled AMD-V (and NX) in BIOS and disabled Hyper-V in Windows.
1. Install VMware from your download.
2. Accept the EULA.
3. No license key should be required for the free version.
