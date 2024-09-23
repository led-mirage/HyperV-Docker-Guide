# How to Run Docker on a Hyper-V Virtual Machine

Created on September 23, 2024  
Copyright (c) 2024 led-mirage  

## Introduction

Running Docker on a Hyper-V virtual machine requires not only installing Docker on the VM, but also configuring certain settings on the host machine. This guide explains how to set up your environment to successfully run Docker inside a Hyper-V virtual machine. Note that this document does not cover the process of creating a Hyper-V virtual machine.

## Testing Environment

- Host Machine: Windows 11 Pro 23H2
    - Hyper-V: Enabled
    - Windows Hypervisor Platform: Disabled
    - Virtual Machine Platform: Disabled
- Guest Machine: Windows 11 Pro 23H2
    - Hyper-V: Disabled
    - Windows Hypervisor Platform: Disabled
    - Virtual Machine Platform: Enabled
    - Docker Desktop 4.33.1 (Running on WSL2)

## Host Machine Configuration

To run Docker inside a Hyper-V virtual machine, you need to enable Nested Virtualization on the host. This allows the virtual machine to use virtualization extensions. To enable Nested Virtualization, run PowerShell as Administrator on the host machine and execute the following command (replace "VM name" with the name of your virtual machine as shown in the Hyper-V Manager):

``` powershell
Set-VMProcessor -VMName "VM name" -ExposeVirtualizationExtensions $true
```

To verify the setting, you can run the following command. If True is returned, Nested Virtualization is successfully enabled:

```powershell
(Get-VMProcessor -VMName "VM name").ExposeVirtualizationExtensions
```

That’s all for the host configuration.

## Installing Docker Desktop

To install Docker Desktop, visit Docker's official website at Docker Download and download the latest version of Docker Desktop.

During installation, leave the Use WSL2 instead of Hyper-V (recommended) option enabled. Docker Desktop can run either using Hyper-V or WSL2, but WSL2 is generally faster and more lightweight. If WSL2 is not already installed, Docker Desktop will take care of the installation.

You will also be prompted to create a Docker account during installation. If you're just using Docker for basic tasks, you can skip this step. However, if you plan on using Docker Hub to upload your own images or use Docker for commercial purposes, you'll need to create an account later.

## Verifying Docker Installation

### Checking the Docker Version

To check if Docker is installed correctly, run the following command in PowerShell or Command Prompt:

```powershell
docker --version
```

You should see an output similar to:

```
Docker version 27.1.1, build 6312585
```

If you see the Docker version information, the installation is successful.

### Running the Hello World Container

To test Docker further, run the following command to execute a simple test container:

```powershell
docker run hello-world
```

This command pulls the hello-world image from Docker Hub, creates a container, and runs it. You should see the following output if everything is working:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
```

This message shows that your installation appears to be working correctly.

## Conclusion

Once you've completed these steps, Docker should be working correctly in your Hyper-V virtual machine. From here, you can pull and run any Docker image you want. For more information about using Docker, there are plenty of resources available online.
