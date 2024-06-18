# Manifest Notes
1. A mainifest file is a scoop manifest file. See [Scoop](https://scoop.sh), the Scoop [wiki](https://github.com/ScoopInstaller/Scoop/wiki) and, most specifically, the description of [Scoop Manifest Files](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests).
2. In this scoop bucket, each manifest file must be versioned. The versioning follows the [Semantic Versioning 2.0](https://semver.org) scheme. See the discussion in repository [instrument-installer](https://github.com/cellanome/instrument-installer/tree/main/design-documentation) for full details.
3. A working knowledge of how Scoop Manifest files work is assumed.
4. A working knowledge of how the Cellanome system operates is essential.
+ Note to the reader: As you read the notes about each manifest, look at the contents of the manifest file. :)


For example:
```
[basename].x.y.z.-[optional build].json
```
Note that the `[basename]` of the manifest file, **SHOULD NOT CHANGE**.  This is due to the fact that the installation script references some manifests for installation processing steps. If you do wish to change a manifest name, the `Install.ps1` script *may need to be updated* to remain in-sync. For example, the installation script needs to know name of the Analysis Service manifest, (named `analysis_service.x.y.z-[b]`) so that it can create the correct path to the executable for launching.


## Manifest File Details

Each Manifest is described below in detail about what it contains and does.
Fields of interest in each manifest are pointed out with details regading anything special about the manifest and the artifact it installs.

Manifests:
 + [System Manifest: `cellanome-system.x.y.z`](#system-Manifest-cellanome-systemxyz)
 + [Component Manifest: `analysis_service.x.y.z-[b]`](#component-Manifest-analysis_servicexyz-b)
 + [Component Manifest: `cellanome_vcredist.x.y.z-[b]`](#component-Manifest-cellanome_vcredistxyz-b)
 + [Component Manifest: `cellanome-cuda-toolkit.x.y.z-[b]`](#component-Manifest-cellanome-cuda-toolkitxyz-b)
 + [Component Manifest: `cellanome-cudnn.x.y.z-[b]`](#component-Manifest-cellanome-cudnnxyz-b)
 + [Component Manifest: `cellanome-folder.x.y.z-[b]`](#component-Manifest-cellanome-folderxyz-b)
 + [Component Manifest: `cellanome-live-navigator.x.y.z-[b]`](#component-Manifest-cellanome-live-navigatorxyz-b)
 + [Component Manifest: `cellanome-model-weights.x.y.z-[b]`](#component-Manifest-cellanome-model-weightsxyz-b)
 + [Component Manifest: `cellanome-moxa.x.y.z-[b]`](#component-Manifest-cellanome-moxaxyz-b)
 + [Component Manifest: `cellanome-vimba-apps.x.y.z-[b]`](#component-Manifest-cellanome-vimba-appsxyz-b)
 + [Component Manifest: `cellanome-vio-mgr-simulator.x.y.z-[b]`](#component-Manifest-cellanome-vio-mgr-simulatorxyz-b)
 + [Component Manifest: `cellanome-vips.x.y.z-[b]`](#component-Manifest-cellanome-vipsxyz-b)
 + [Component Manifest: `cellanome-wsl-instrument.x.y.z-[b]`](#component-Manifest-cellanome-wsl-instrumentxyz-b)
 + [Component Manifest: `cellanome-zlib.x.y.z-[b]`](#component-Manifest-cellanome-zlibxyz-b)
 + [Component Manifest: `microscope-gui-package.x.y.z-[b]`](#component-Manifest-microscope-gui-packagexyz-b)

<br>

### System Manifest: `cellanome-system.x.y.z`

This manifest is the top-level system manfiest file. While every Manifest must have an artifact to install, the artifect specifed in the `url` section of this manifest only points to a dummy file and the contents of that artifact can be anything.

This manifest has two special sections that need to be maintained:
- the `depends` field
- the `suggest` field

#### System manifest `depends` field
This field must contain a list of manifest files that specify all the **required** components to be installed for the system.  **These mainifests will always be installed by the installer script.**  Additionally, it is imporatant to remember to have the correct scoop bucket name in the child items (i.e. `internal` or `commercial`).



#### System manifest `suggest` field
This field should always contain the manifest for the Vio Manager Simulator *that is compatible with the given system manifest*.  This component will be installed by the main installer script only if the user confirms its installation.  Additionally, it is imporatant to remember to have the correct scoop bucket name in the child item (i.e. `internal` or `commercial`).
+ As of this writing, operation of both the simulation verion of the Vio Manager and the real Vio Manager is unknown at this time.
+ *TBD - get and install the real Vio Manager.*

<br>

### Component Manifest: `analysis_service.x.y.z-[b]`

Installs the Analyis Service

`url`: a zip of the anaylsis service executable and the dependent folders needed to run.
`extract_dir`: the name of the top level folder in the zip file.
`bin`: the name of the executable that will be scoop "shimmed".

<br>

### Component Manifest: `cellanome_vcredist.x.y.z-[b]`

Installs the Microsoft VC++ redistributables (from a copy on the Cellanome storage).

`url`: a zip of the widely available MS VC++ runtime for Visual Studio 2022.
`bin`: the name of the executable that will be scoop "shimmed".
`post-install`: Since the downloaded package is an installer itself, this section are the commands to run the installer from the command line.

<br>

### Component Manifest: `cellanome-cuda-toolkit.x.y.z-[b]`

Installs the Nvidia CUDA Toolkit

Special note: this manifest is a direct copy of the publically available Nvidia Cuda Toolkit scoop installer from the Scoop [Versions](https://github.com/ScoopInstaller/Versions) bucket.  The contents should not change.  Also note that this manifest retrieves the Cuda Toolkit artifact from the Nvidia storage and not the Cellanome storage.

<br>

### Component Manifest: `cellanome-cudnn.x.y.z-[b]`

Installs the Nvidia CUDA Deep Neural Network (from a copy on the Cellanome storage).

`url`: a copy of Nvidia's CUDNN zipfile.
`extract_dir`: the name of the top level folder in the zip file.
`post_install`: updates system PATH environment variable to include the bin folder of the extracted folders.

<br>

### Component Manifest: `cellanome-folder.x.y.z-[b]`

Installs Cellanome configuration files.

`url`: a zip of the supported `.cellanome` folder.
`extract_dir`: the name of the top level folder in the zip file.
`post_install`: copies the extracted `.cellanome` folder to the current users home folder.

<br>

### Component Manifest: `cellanome-live-navigator.x.y.z-[b]`

Installs Cellanome Flowcell Live application

`url`: the build artifact from the live-navigator repository.
`installer`: embedded (Powershell) script commands to run the Flowcell Live application installer.

<br>

### Component Manifest: `cellanome-model-weights.x.y.z-[b]`

Installs Cellanome Model Weights used by the analysis processes.  Note that during the post install section of the main Install script, the path to this file will be updated in the `instrument_configuration.json` file.

`url`: a zip of the model weights file.

<br>

### Component Manifest: `cellanome-moxa.x.y.z-[b]`

Installs the MOXA UPort 1110/1130/1150 Windows Driver (from a copy on the Cellanome storage).

`url`: a zip of the MOXA installer executalbe. (It is zipped due to Scoop needing to compress executables.)
`installer`: embedded (Powershell) commands on how to run the installer extracted from the zip.

<br>

### Component Manifest: `cellanome-vimba-apps.x.y.z-[b]`

Installs the Vimba USB Transport Layer driver and the Vimba support apps (from a copy on the Cellanome storage).

`url`: a zip of **extracted** msi installers from the publicly available Vimba installer.
`installer`: embedded (Powershell) instructions on how to run the installers extracted from the zip.
+ In this case, the msi installers are run **and** the `.inf` files installed by the msi installer are installed via the Windows [pnputil](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/pnputil) tool.
+ Should the need to extract the msi installers from the publicly available Vimba installer again arise, run the command `Vimba_v6.0_Windows.exe /extract`.  This will open a GUI window to allow you to specify where to extract the msi files. Currently, only two msi files are used: one to install the Vimbar Transport Layer drivers and another to install the Vimba application (viewer, firmware updater, and driver installer).

<br>

### Component Manifest: `cellanome-vio-mgr-simulator.x.y.z-[b]`

Installs the Venture IO Manager (hardware) Simulator

`url`: zip of `igloo_test_suite-0.1.1-py3-none-any.whl`, `requirements.txt` and `requirements_cellanome.txt` files. See the [igloo_test_suit](https://github.com/cellanome/igloo_test_suite) github repository for details.
+ Durring the post install section of the main Install script is where the python virtual environment is created.
+ This package may or may not be installed as it's installation is controlled by user input.

<br>

### Component Manifest: `cellanome-vips.x.y.z-[b]`

Installs the [libvips](https://www.libvips.org/) image processing library (from a copy on the Cellanome storage).

`url`: a copy of the libvips artifact. Available [here](https://github.com/libvips/libvips/releases).  Uses the "Windows binaries" and the x64 "web" version. For example, the archive named `vips-dev-w64-web-8.15.2.zip`.
`extract_dir`: the name of the top level folder in the zip file.
`post_install`: updates system PATH environment variable to include the bin folder of the extracted folders.

<br>

### Component Manifest: `cellanome-wsl-instrument.x.y.z-[b]`

Installs the Cellanome WSL linux distro image and AWS credentials folder.

`url`: a zip of the Instrument tarball and the current AWS credentials folder used by the WSL instance.
+ the actuall creation and configuration of the WSL instance (via the import of the tarball) occurs during the post install section of the main installer script.
+ linkage to the AWS credentials folder also occurs during the post install section of the main installer script.

<br>

### Component Manifest: `cellanome-zlib.x.y.z-[b]`

Installs the [zlib](https://www.zlib.net/) compression library (from a copy on the Cellanome storage).

`url`: a zip of the zlib artifact
`post_install`: updates system PATH environment variable to include the dll_x64 folder of the extracted folders.

<br>

### Component Manifest: `microscope-gui-package.x.y.z-[b]`

Installs the Cellanome Microscope GUI Controller (backend) application.

`url`: a zip of the Cellanome Microscope GUI Contrller artifact
`extract_dir`: the name of the top level folder in the zip file.
`bin`: the name of the executable that will be scoop "shimmed".
