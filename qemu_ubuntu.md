# Install QEMU on OSX

QEMU requires brew in OSX, so we need to install brew first. 

## Installing Brew
To install brew we need to have the developer tools enabled in our system. In order to install those tools, we have two options.

 1. Download Xcode form the AppStore
 2. In your terminal run the following command: `xcode-select --install`

Once you have the developer tools, run the following command in your terminal to install brew:
```
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Once the installation is finished, we can test the brew installation in our terminal with the command 
```
$ brew doctor
```

## Installing QEMU
Now that we have brew on the system we can proceed to the QEMU installation. To do that we use a simple command:
```
$ brew install qemu
```

When the installation is finished, we can run the following command to test if the installation was successful:
```
$ qemu-system-x86_64 --version
```

# Installing Ubuntu on QEMU
We need to follow a series of steps in order to install an Ubuntu VM on QEMU. The first one is go to the Ubuntu website and download an `.iso` file of the operating system.
Once we have the `.iso`, we need to to this:

 1. Create a folder in your computer (with any name and anywhere). `$ mkdir ubuntu_vm`
 2. Move the `.iso` file in the folder that you just create. `$ mv ubuntu.iso ./your-folder`
 3. Then move inside the folder you just create `$ cd ubuntu_vm`
 4. Inside the folder, create the drive file of the system `qemu-img create -f qcow2 ubuntu_drive.qcow2 10G`
 5. Run `ls` to verify that the .iso and the qcow file are inside the folder

Once you have the previous steps, you can run the following command to run the iso and complete the installation:
```
qemu-system-x86_64 \
  -m 2048 \
  -vga virtio \
  -show-cursor \
  -usb \
  -device usb-tablet \
  -enable-kvm \
  -cdrom your_ubuntu.iso
  -drive file=ubuntu_drive.qcow2,if=virtio \
  -accel hvf \
  -cpu host
```
The previous command, will run the iso, when you are there, you need to complete the ubuntu installation, so once you see something you must:

 1. Click on Install Ubuntu 
 2. Select `erase disk` when the installer ask about it
 3. When the installer asks you to restart the machine close the QEMU window.

Now, in order to make more easy to run our QEMU VM, we are going to create a shell command. So, lets do the following.

First, create a new file in any place you want:
```
$ vi ubuntuvm.sh
```

Then, paste the following and adept it to your needs:
```
qemu-system-x86_64 \
  -m 2048 \
  -vga virtio \
  -show-cursor \
  -usb \
  -device usb-tablet \
  -enable-kvm \
  -cdrom your_ubuntu.iso
  -drive file=~/absolute/path/to/your/ubuntu_drive.qcow2,if=virtio \
  -accel hvf \
  -cpu host
```

And finally, write and quit.

As you can see, the command deletes the instruction about the .iso file. This is because now we have the operating system already installed. 
Now in order to run the vm, we need to make our file executable. So run the following command:
```
$ chmod +x ubuntuvm.sh
```

And finally to run it:
```
$ ./ubuntuvm
```

And that is it. We should see our virtual machine running correctly.