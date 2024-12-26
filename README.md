# wsl-5.15.90.1-prebuilt
A prebuilt WSL kernel to use with PINTO's wsl2_linux_kernel_usbcam_enable_conf

To passthrough and actually use WebCams with WSL2, you need to rebuild a WSL kernel with a lot of configurations made different from the standard WSL2 kernel. Luckily, PINTO0309's repository with the configuration files needed and information on how to build the kernel make it a lot easier. The point of this repository is to offer a prebuilt 5.15.90.1 kernel, which you'll need to use PINTO's configuration file.

I'll also instruct you in using usbipd on Windows PowerShell, which you'll need to passthrough the USB. You can find PINTO0309's repo here:

https://github.com/PINTO0309/wsl2_linux_kernel_usbcam_enable_conf/



You'll need to create a folder for your WSL kernel. I used 'C:\WSL\CustomKernel'.

You'll then need to edit the .wslconfig file in your users folder ('C:\Users\<YourUsername>')

The file should look like this:

[wsl2]
kernel=C:\\WSL\\CustomKernel\\bzImage


You need to create and edit the .wslconfig file to include this.

Then run 'wsl --shutdown' and 'wsl -d Ubuntu-22.04' to boot WSL.

As PINTO0309's repository say, the version of Ubuntu installed has to be Ubuntu-20.04 or Ubuntu-22.04.

The double backslashes are a programmatic way to call Windows' paths.

Once you reopen WSL by using 'wsl -d', you should see '5.15.90.1-microsoft-standard-WSL2+' when running 'uname -r -v'.

From here you can follow PINTO0309's instructions.


To use usbipd, you'll need to install it in PowerShell:

'winget install --interactive --exact dorssel.usbipd-win'

Then run 'usbipd list' to list your USB devices. After that, you'll need to use your USB device BUSID to get it bound and attached. For example, for this case:

BUSID  VID:PID    DEVICE                                                        STATE
2-1    04d9:1503  Dispositivo de Entrada USB                                    Not shared
2-8    046d:c077  Dispositivo de Entrada USB                                    Not shared
2-14   1b3f:2247  GENERAL WEBCAM                                                Not shared
2-19   0bc2:ab24  Dispositivo de Armazenamento em Massa UAS (USB Attached S...  Not shared

I would need to run 'usbipd bind --busid 2-14' on a Admin privileged PowerShell to bind it and then run 'usbipd attach --wsl Ubuntu-22.04 --busid 2-14' to attach it to USB/IP, which will make it usable in WSL.

It should look like this after attached:

BUSID  VID:PID    DEVICE                                                        STATE
2-1    04d9:1503  Dispositivo de Entrada USB                                    Not shared
2-8    046d:c077  Dispositivo de Entrada USB                                    Not shared
2-14   1b3f:2247  GENERAL WEBCAM                                                Attached
2-19   0bc2:ab24  Dispositivo de Armazenamento em Massa UAS (USB Attached S...  Not shared

After following PINTO0309's instructions, you should get your webcam working. Keep in mind that this will only work for external webcams and that some webcams may not be supported.