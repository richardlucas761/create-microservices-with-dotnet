# create-microservices-with-dotnet

Experiments and *stream-of-consciousness* thoughts with the "Create cloud-native apps and services with .NET and ASP.NET Core" learning path on Microsoft Learn.

<https://learn.microsoft.com/en-gb/training/paths/create-microservices-with-dotnet/>

Limited success with this so far after trying various options. There is probably a "cloud native development" learning curve here which is complicating things, although I would be surprised if modern developers are all using Linux to develop rather than Windows 11 so it must be *possible*, it's just proving very difficult to get the Docker container to launch.

## VS Code versus VS Codium

I started out using [VS Codium](https://github.com/VSCodium/vscodium) but had some issues with the *Dev Containers* extension alternative in VS Codium so switched to VS Code for this learning as the path of least resistance or closest to the Microsoft instructions I'm following.

## Various options attempted

### 1. GitHub Codespaces

<https://github.com/features/codespaces> "Codespaces is free for individual use up to 60 hours a month and comes with simple, pay-as-you-go pricing after that." and it's not clear how I could share my experiments in this environment publicly?

I was expecting the environment to be already set up but this command after rebuilding the container as prompted failed in the Terminal?

```
cd ./dotnet-docker 
 dotnet publish /p:PublishProfile=DefaultContainer
 ```

 ```
 bash: dotnet: command not found
 ```

 *This* note suggests both Docker and .NET SDK should be present in the GitHub Codespace and you would have hoped the "pre configured" option would have both of them but not sure how to check this is actually installed?

 ```
 You can complete this exercise in an instance of GitHub Codespaces that has Docker and the .NET SDK preinstalled. When you use these tools and techniques in your own development environment, make sure that you have these prerequisites installed.
 ```

### 2. Running locally in VS Code on Windows 11

Fatal errors were encountered getting this running in VS Code on Windows 11 which are detailed below.

#### Fork of mslearn-dotnet-cloudnative

The learning path suggests cloning this repo:

<https://github.com/MicrosoftDocs/mslearn-dotnet-cloudnative>

But I chose to create my own fork instead so I can tinker with the code:

<https://github.com/richardlucas761/mslearn-dotnet-cloudnative>

#### System Requirements for running Dev Container

I needed to install *Docker Desktop* and also *WSL 2*.

I have various versions of PowerShell installed on my PC and *this* command didn't work in the *Visual Studio 2026 Developer PowerShell* but did work in *Windows PowerShell* when running as Administrator, go figure.

```wsl --install```

#### Dev Containers extension and the alternative for VS Codium

The "Developing inside a Container" page here <https://code.visualstudio.com/docs/devcontainers/containers> asks me to install the [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension in VS Code.

This would be a simple one-click install from the Marketplace website if I wasn't using [VS Codium](https://github.com/VSCodium/vscodium) rather than VS Code.

The suggestion seems to be to use this extension instead in VSCodium <https://open-vsx.org/extension/s-h-a-d-o-w/dev-containers-oss> but I couldn't get that working, will come back to that later possibly.

#### >Dev Containers: Rebuild and Reopen in Container

Attempting to create the development container locally *this* information message was displayed, will assume this isn't a hard requirement and chose to "Continue".

```
[Window Title]
Visual Studio Code

[Main Instruction]
Not all host requirements in devcontainer.json are met by the Docker daemon.

[Content]
Docker daemon: CPUs 24, RAM 7.63gb, GPU
devcontainer.json: CPUs 4, RAM 8gb

[Continue] [Cancel]
```

#### "An error occurred setting up the container."

This error was displayed with some debugging information in the log but it's not clear which of the messages would be useful to include here.

I chose to "Edit devcontainer.json Locally" and alter the memory requirements from 8GB to 7GB but this did not resolve the issue.

```
[Window Title]
Visual Studio Code

[Content]
An error occurred setting up the container.

[Retry] [Edit devcontainer.json Locally] [More Actions...]
```

#### .devcontainer\dotnet-docker\devcontainer.json

The *devcontainer.json* file includes these extensions which weren't installed in VS Code but installing these didn't fix the issue.

```
    "customizations": {
        "vscode": {
            "extensions": [
                "ms-vscode.vscode-node-azure-pack",
                "github.vscode-github-actions",
                "ms-dotnettools.csdevkit"
            ]
        }
    },
```

#### docker-outside-of-docker error

The log file contains this error which is a little garbled but suggests I can look here to troubleshoot the error as "command not found" implies there is something missing from the local installation.

<https://github.com/devcontainers/features/blob/main/src/docker-outside-of-docker/README.md>

The README mentions the "ms-azuretools.vscode-containers" extension so this was installed into VS Code but this didn't resolve the issue.

```
[2026-10-26T11:01:38.724Z] 
0.158 ./install.sh: line 118: err: command not found
[2026-10-26T11:01:38.724Z] 
0.158 ERROR: Feature "Docker (docker-outside-of-docker)" (ghcr.io/devcontainers/
/features/docker-from-docker) failed to install! Look at the documentation at htt
tps://github.com/devcontainers/features/tree/main/src/docker-outside-of-docker fo
[2026-10-26T11:01:38.725Z] 
or help troubleshooting this error.
[2026-10-26T11:01:38.725Z] 
```

### 3. Running just the API locally in Visual Studio on Windows 11

Modify *mslearn-dotnet-cloudnative\dotnet-docker\Products\appsettings.json* to use a local database and just run the Products API locally, ignoring the complication of Docker completely, but this kind of defeats the object of the lesson. (This was a lot simpler when OpenShift did a lot of this for me!)

### 4. Running locally in VS Code on Linux Mint (dual boot)

To be attempted, dual boot install on my main PC is running on a spinning rust drive so it's *painfully* slow to use and this is probably a non starter.

### 5. Running locally in VS Code on Linux Mint / Ubuntu VM

Why not just use Ubuntu as that's already set up on this PC for WSL? *TBC.*

### 6. Running locally in VS Code on Linux Mint running in Virtual Box

Virtual Box is the virtualisation tool I've used the most and am most comfortable with, but there was a limitation here of a lack of virtualisation support inside the Virtual Box client. Inception, anyone?
