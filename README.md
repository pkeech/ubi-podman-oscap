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
This Image is based upon the Red Hat Universal Base Image (UBI) 8 and includes `Podman` and `OpenSCAP`. 

> **Note:** Normally, OpenSCAP is only available in a Red Hat or CentOS repo. In this Image, the `RPM` packages are pulled from an Online repo and installed manually.

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
Use the following steps to build and run this Docker Image.

> **Note:** This image **DOES NOT** include benchmarks. To obtain benchmarks, please review the section below.

1. Build the Docker image and tag `docker build src -t << IMAGE-NAME >>`
2. Create a Temporary Docker Volume `docker volume create podman-temp`
3. Run the Docker Image `docker run -it --rm --privileged -v podman-temp:/var/lib/containers << IMAGE-NAME >>`
4. Pull the Image you wish to scan. e.g. `podman pull ubuntu:20.04`
5. Scan the Image. e.g. `oscap-podman ubuntu:20.04 xccdf eval --verbose ERROR --fetch-remote-resources --profile xccdf_org.ssgproject.content_profile_stig scap-content/scap-security-guide-0.1.58/ssg-ubuntu2004-ds.xml`

### Usage (Gitlab)
Use the following steps as a guide to create a Gitlab CI stage.

1. *Placeholder*

### Obtaining Benchmarks
This image doesn't contain any benchmarks. A great resource for benchmarks if provided by the great folks that contribute [ComplianceAsCode](https://github.com/ComplianceAsCode/content)

To quickly add benchmarks to the image, run the following commands:

1. Start the OpenSCAP container

``` bash
docker run -it --rm --privileged -v podman-temp:/var/lib/containers << IMAGE-NAME >>
```

2. Download the Benchmarks and extract

``` bash
## DOWNLOAD BENCHMARKS
curl -L https://github.com/ComplianceAsCode/content/releases/download/v0.1.58/scap-security-guide-0.1.58.zip -o scap-security-guide.zip

## EXTRACT BENCHMARKS
unzip -qq -o "scap-security-guide.zip" -d "scap-content"

## CLEANUP
rm -rf /OpenSCAP/scap-security-guide.zip
```

3. View Profiles of Benchmark. Change the `xml` file to match the Benchmark you wish to use.

``` bash
oscap info --profiles scap-content/scap-security-guide-0.1.58/ssg-ubuntu2004-ds.xml
```

4. Verify

``` bash
## PULL TEST IMAGE (UBUNTU 20.04)
podman pull ubuntu:20.04

## SCAN TEST IMAGE
oscap-podman ubuntu:20.04 xccdf eval --verbose ERROR --fetch-remote-resources --profile xccdf_org.ssgproject.content_profile_stig scap-content/scap-security-guide-0.1.58/ssg-ubuntu2004-ds.xml
```

### Changes & Updates
View the [Changelog](https://github.com/pkeech/ubi-podman-oscap/blob/main/CHANGELOG.md)

### Notes
Listed below are notes about this image:

* Currently, this image requires the `--privileged` tag. Another image will be created to remove this requirement.
* You will need to include the required benchmarks to perform a scan.
* A volume needs to be mapped to `/var/lib/containers`. This appears to be a requirement for `podman`.