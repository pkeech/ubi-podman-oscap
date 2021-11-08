## DEFINE BASE IMAGE
FROM registry.access.redhat.com/ubi8:8.4

## COPY CUSTOM REPOS
COPY custom.repo /etc/yum.repos.d/custom.repo

## UPDATE IMAGE REPOS
RUN sed -i "s/enabled=1/enabled=0/" /etc/dnf/plugins/subscription-manager.conf && \
    chmod 644 /etc/yum.repos.d/custom.repo && \
    rm -f /etc/yum.repos.d/ubi.repo && \
    dnf repolist && \
    dnf update -y

## UPDATE BASE IMAGE WITH DEPENDS
RUN dnf install podman openscap-scanner -y

## DOWNLOAD OSCAP-PODMAN TOOL & DEPENDS
RUN curl -L https://rpmfind.net/linux/centos/8-stream/AppStream/x86_64/os/Packages/openscap-utils-1.3.5-2.el8.x86_64.rpm -o openscap-utils-1.3.5-2.el8.x86_64.rpm && \
    curl -L https://rpmfind.net/linux/centos/8-stream/AppStream/x86_64/os/Packages/openscap-1.3.5-2.el8.x86_64.rpm -o openscap-1.3.5-2.el8.x86_64.rpm && \
    curl -L https://rpmfind.net/linux/centos/8-stream/AppStream/x86_64/os/Packages/openscap-scanner-1.3.5-2.el8.x86_64.rpm -o openscap-scanner-1.3.5-2.el8.x86_64.rpm && \
    curl -L https://rpmfind.net/linux/centos/8-stream/AppStream/ppc64le/os/Packages/rpmdevtools-8.10-8.el8.noarch.rpm -o rpmdevtools-8.10-8.el8.noarch.rpm

## INSTALL OSCAP-PODMAN TOOL & DEPENDS
RUN dnf install rpmdevtools-8.10-8.el8.noarch.rpm openscap-scanner-1.3.5-2.el8.x86_64.rpm openscap-1.3.5-2.el8.x86_64.rpm openscap-utils-1.3.5-2.el8.x86_64.rpm -y

## CREATE OPENSCAP WORKING DIRECTORY
WORKDIR /OpenSCAP

## DOWNLOAD SCAP CONTENT AND EXTRACT
RUN curl -L https://github.com/ComplianceAsCode/content/releases/download/v0.1.58/scap-security-guide-0.1.58.zip -o scap-security-guide.zip && \
    unzip -qq -o "scap-security-guide.zip" -d "scap-content"

## SWTICH TO BASH SHELL
SHELL ["/bin/bash", "-c"]

## TODO: CREATE ENTRYPOINT SCRIPT THAT WILL PULL THE IMAGE PRIOR TO RUNNING SCAN
## DEFINE ENTRYPOINT SCRIPT
#ENTRYPOINT ["oscap-podman"]
ENTRYPOINT ["/bin/bash"]

## EXAMPLE ENTRYPOINT
# oscap-podman ubuntu:20.04 xccdf eval --verbose ERROR --fetch-remote-resources --profile xccdf_org.ssgproject.content_profile_stig scap-content/scap-security-guide-0.1.58/ssg-ubuntu2004-ds.xml
# oscap-podman docker.io/library/ubuntu:20.04 xccdf eval --verbose ERROR --fetch-remote-resources --profile xccdf_org.ssgproject.content_profile_stig scap-content/scap-security-guide-0.1.58/ssg-ubuntu2004-ds.xml