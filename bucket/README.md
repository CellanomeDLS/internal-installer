# Manifest Notes
1. In this bucket, each manifest must be versioned.
For example:
```
[basename].x.y.z.json
```
2. The base name of the manifest file up to the version number, **MUST NOT CHANGE**.  This is due to the fact that the installation script needs to have well known package names durring some post scoop installation processing steps. If you do wish to change it,
the Install.ps1 script *may need to be updated too*.
3. The top-level system manfiest file, `cellanome-system.x.y.z`, has two special sections that need to be maintained:
- the `depends` field
- the `suggest` field

### System manifest `depends` field
This field must contain a list of manifest files that specify all the **required** packages to be installed.  These mainifests will always be installed by the installer script.
### System manifest `suggest` field
This field should always contain the manifest for the Vio Manager simulator *that is compatible with the given system manifest*.  This package will be installed by the intaller script only if the user confirms its installation.
