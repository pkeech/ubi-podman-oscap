<!-- PROJECT LOGO -->
<br />
<div align="center">

  <a href="https://github.com/pkeech/ubi-podman-oscap/">
    <img src="https://github.com/pkeech/ubi-podman-oscap/blob/main/docs/images/OpenSCAPBanner.png?raw=true" alt="Logo" width="480" />
  </a>

  <p align="center">
    <a href="https://github.com/pkeech/ubi-podman-oscap/-/issues">Report Bug</a>
    ·
    <a href="https://github.com/pkeech/ubi-podman-oscap/-/issues">Request Feature</a>
  </p>
  
</div>

### Description
This Image is based upon the Red Hat Universal Base Image (UBI) 8 and includes `Podman` and `OpenSCAP`. **Note:** Normally, OpenSCAP is only available in a Red Hat or CentOS repo. In this Image, the `RPM` packages are pulled from an Online repo and installed manually.

View the [Changelog](https://github.com/pkeech/ubi-podman-oscap/blob/main/CHANGELOG.md)

### Compatibility
The following table shows the current benchmarks:

| Operating System / App Name | Versions |
| --- | :---: |
| CentOS | 7, 8 |
| Chromium | - |
| Debian | 9, 10 |
| Fedora | - |
| Firefox | - |
| Fuse | 6 |
| JRE | - |
| MacOS | 10.15 |
| OCP4 | - |
| Oracle Linux (OL) | 7, 8 |
| OpenSUSE | - |
| Red Hat Container OS (RHCOS) | 4 |
| Red Hat Enterprise (RHEL) | 7, 8, 9 |
| Red Hat OpenStack (RHOSP) | 10, 13 |
| Red Hat Virtualization (RHV) | 4 |
| Scientific Linux (SL) | 7 |
| SUSE (SEL) | 12, 15 |
| Ubuntu | 16.04, 18.04, 20.04 |
| McAfee VSE | - |
| Wind River (WRLinux) | 10.19, 8 |

### Usage (Standalone)
Use the following steps to build and run this Docker Image. Additionally, this image can be used within a CI/CD Pipeline.

1. Build the Docker image and tag `docker build src -t << IMAGE-NAME >>`
2. Create a Temporary Docker Volume `docker volume create podman-temp`
3. Run the Docker Image `docker run -it --rm --privileged -v podman-temp:/var/lib/containers << IMAGE-NAME >>`


### Notes
Listed below are notes about this image:

* Currently, this image requires the `--privileged` tag. Another image will be created to remove this requirement.
* You will need to include the required benchmarks to perform a scan.
* A volume needs to be mapped to `/var/lib/containers`. This appears to be a requirement for `podman`.