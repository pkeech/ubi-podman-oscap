## DEFINE BASE IMAGE FOR STANDARD IMAGE
FROM registry.access.redhat.com/ubi8:8.4 as standard

## CREATE OPENSCAP WORKING DIRECTORY
WORKDIR /OpenSCAP

## COPY CUSTOM REPOS
COPY custom.repo /etc/yum.repos.d/custom.repo

## UPDATE IMAGE REPOS & UPDATE ENTRYPOINT
RUN sed -i "s/enabled=1/enabled=0/" /etc/dnf/plugins/subscription-manager.conf && \
    chmod 644 /etc/yum.repos.d/custom.repo && \
    rm -f /etc/yum.repos.d/ubi.repo && \
    dnf repolist && \
    dnf update -y

## DOWNLOAD OSCAP-PODMAN TOOL & DEPENDS, INSTALL AND CLEANUP
RUN dnf install podman openscap-scanner -y && \
    curl -L https://rpmfind.net/linux/centos/8-stream/AppStream/x86_64/os/Packages/openscap-utils-1.3.5-2.el8.x86_64.rpm -o openscap-utils-1.3.5-2.el8.x86_64.rpm && \
    curl -L https://rpmfind.net/linux/centos/8-stream/AppStream/x86_64/os/Packages/openscap-1.3.5-2.el8.x86_64.rpm -o openscap-1.3.5-2.el8.x86_64.rpm && \
    curl -L https://rpmfind.net/linux/centos/8-stream/AppStream/x86_64/os/Packages/openscap-scanner-1.3.5-2.el8.x86_64.rpm -o openscap-scanner-1.3.5-2.el8.x86_64.rpm && \
    curl -L https://rpmfind.net/linux/centos/8-stream/AppStream/ppc64le/os/Packages/rpmdevtools-8.10-8.el8.noarch.rpm -o rpmdevtools-8.10-8.el8.noarch.rpm && \
    dnf install rpmdevtools-8.10-8.el8.noarch.rpm openscap-scanner-1.3.5-2.el8.x86_64.rpm openscap-1.3.5-2.el8.x86_64.rpm openscap-utils-1.3.5-2.el8.x86_64.rpm -y && \
    dnf clean all && \
    rm -rf /var/cache/dnf/ /var/tmp/* /tmp/* /var/tmp/.???* /tmp/.???* openscap-utils-1.3.5-2.el8.x86_64.rpm openscap-1.3.5-2.el8.x86_64.rpm openscap-scanner-1.3.5-2.el8.x86_64.rpm rpmdevtools-8.10-8.el8.noarch.rpm

## DOWNLOAD SCAP CONTENT AND EXTRACT
#RUN curl -L https://github.com/ComplianceAsCode/content/releases/download/v0.1.58/scap-security-guide-0.1.58.zip -o scap-security-guide.zip && \
#    unzip -qq -o "scap-security-guide.zip" -d "scap-content" && \
#    rm -rf /OpenSCAP/scap-security-guide.zip

## DEFINE ENTRYPOINT SCRIPT
ENTRYPOINT ["/bin/bash"]

## EXAMPLE COMMANDS
# oscap-podman ubuntu:20.04 xccdf eval --verbose ERROR --fetch-remote-resources --profile xccdf_org.ssgproject.content_profile_stig scap-content/scap-security-guide-0.1.58/ssg-ubuntu2004-ds.xml
# oscap-podman ubuntu:20.04 xccdf eval --verbose ERROR --fetch-remote-resources --profile xccdf_org.ssgproject.content_profile_stig --results compliance_output_report.xml --report report.html scap-content/scap-security-guide-0.1.58/ssg-ubuntu2004-ds.xml