# Security and Compliance

## OpenSCAP

OpenSCAP is an open-source tool suite for checking systems for security vulnerabilities and ensuring compliance with security policies.

SCAP means *Security Content Automation Protocol*. It's a standard, developed by the US agency NIST.

### SCAP Vocabulary

| Expression | Meaning | Purpose |
| --- | --- | --- |
| **XCCDF** | Extensible Configuration Checklist Description Format | A XML-file that describes what should be checked |
| **OVAL** | Open Vulnerability and Assessment Language | A XML-file that describes how the system should be checked |
| **CVE** | Common Vulnerabilities and Exposures | A standardized name for a known security vulnerability |
| **SSG** | SCAP Security Guide | A gigantic, predefined collection of XCCDF and OVAL files |

### Important tools

- `oscap`: The CLI-tool thats the actual scanner
- SCAP Workbench: A GUI tool to define and to adapt the guidelines
- SCAP Timetail: A tool to compare reports from time to time

### The workflow

To check a server a scanner (`openscap-scanner`) and rules (`scap-security-guide`) are needed.

#### Step 1: Display available profiles

The SSG includes profiles for various standards (e.g., CIS, PCI-DSS, or HIPAA). Here's how to list them (example for RHEL/Alma/Rocky Linux 8):

```bash
oscap info /usr/share/xml/scap/ssg/content/ssg-rhel8-ds.xml
```

#### Step 2: Execute the scan and create a report

This command checks the system against the strict CIS (Center for Internet Security) profile and outputs an extremely detailed, easily readable HTML report:

```bash
oscap xccdf eval \
  --profile xccdf_org.ssgproject.content_profile_cis \
  --report /tmp/security-report.html \
  /usr/share/xml/scap/ssg/content/ssg-rhel8-ds.xml
```

### OpenSCAP and Ansible (Remedation)

!!! tip "OpenSCAP is able to create an Ansible playbook automatically out of failed checks, that fixes exactly this error."

To generate a playbook from a scan:

```bash
oscap xccdf generate fix \
  --fix-type ansible \
  --profile xccdf_org.ssgproject.content_profile_cis \
  --output /tmp/fix-my-server.yml \
  /usr/share/xml/scap/ssg/content/ssg-rhel8-ds.xml
```

!!! warning "Never blindly execute generated remediation playbooks on production systems! The automatic fixes deeply affect the system (e.g., disabling SSH root logins, changing file permissions). Always review the playbook beforehand and test it on a staging system."
