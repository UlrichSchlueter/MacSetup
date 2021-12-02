# ARO MacOS Initial Setup


## Required Components


### Prepare
Install required components, namely homebrew and ansible. There are many ways to install python on MacOS, I prefer the homebrew way. 

Run: 
```
./1_installHomeBrew.sh
./2_installPythonAndAnsible.sh
```

## Install things

Run:
```
./3_runAnsiblePlaybook.sh
```

This will install required pieces of software for ARO, Visual Studio Code and also create a link to redirect docker to podman. Some commands like the azure cli require this for now (Nov 2021) (see [https://github.com/Azure/azure-cli/issues/14768], might be fixed by now)

> Please note that this will fail for M1 Macs, because the path to podman is different, but I don't have an M1 Mac try.


## Manual Config: 

Most ARO scripts expect a Linux environment with all the Gnu tools present. The playbook installed the Mac version of the gnu tools for you. What's left is to make sure that they are found first your $PATH before the Mac native ones. Put a line like the following into .zshrc or your prefered place.  

.zshrc
```
export PATH="/usr/local/bin:/usr/local/opt/coreutils/libexec/gnubin:/usr/local/opt/findutils/libexec/gnubin:/usr/local/opt/grep/libexec/gnubin:$PATH"
```


## Podman

(for details see [https://www.redhat.com/sysadmin/podman-mac-machine-architecture])

```
podman machine init \
    --cpus 6 \
    --memory 8096 \
    --disk-size 50GB
```

The ARO components need quite a bit of memory to compile, so give podman some horsepower. 




### Docker For Desktop vs Podman

Both Docker for Desktop and Podman create a virtual machine and execute the container command within that machine, trying their best to hide this from you for best user experience. However that means some things are difficult to implement and are not available:

|Feature | Docker For Desktop | Podman |
|--------|--------------------| -------|
| Mount volumes | Supported | Not supported yet|
| Unix Socket connections | Not Supported | Not supported yet|
| Licence Cost | Yes | No|


## Virtual Machine Mode

Due to the restrictions mentioned, some things will require to work in a local or remote VM. Moving to Virtual Machine mode development (installing Fedora in a Virtual Box VM ) completely. Due to the resource consumption of an additional system this may or may not work for you. A setup where you run Visual Studio Code in native mode and develop via an ssh connection using the [Remote ssh plugin](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh ) might be a good compromise.
