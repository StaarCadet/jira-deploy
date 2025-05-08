# Atlassian JIRA Ansible Playbooks and Certificate Configuration

This repository contains a fully automated Atlassian Jira deployment, complete with SSL. Make sure you check the vars file for any pertinent variables for your environment. Additionally, make sure you check roles/jira_certconfig/templates/frag2.xml for any changes you may need to make for the server.xml assembly.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Jira Install Vars](#Jira-Install-Vars)
3. [Linux Cert Config Vars](#Linux-Cert-Config-Vars)
4. [Notes](#notes)

### Prerequisites

* SSL-ready domain name (e.g. jira.network.com)
   + Set in DNS mapped to target IP/hostname
* Ansible 2.9 or higher
   + Ansible-Galaxy collections:
      + community.general
      + community.crypto
* Python 2.7 or higher
* A functional Ansible inventory
* A Certificate Authority or PKI server
* Appropriate certificate generation permissions for the provided `ad_username` and `ad_password`

### Jira Install Vars

| Variable               | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| `appprops`            | Path to the JIRA application properties file                  |
| `serverxml`           | Path to the Tomcat server configuration file                  |
| `setenv`              | Path to the JIRA environment file                             |
| `share`               | Path to the NFS share                                         |
| `current`             | Path to the current JIRA directory                           |
| `installation`        | Path to the JIRA installation directory                       |
| `link`                | Path to the symbolic link for the current JIRA version        |
| `tarfile`             | Name of the JIRA tarball                                     |
| `foldername`          | Name of the extracted JIRA directory                          |
| `jarfilepath`         | Path to the JIRA JAR files                                   |
| `jdbcfile`            | Name of the JDBC driver JAR file                             |
| `version`             | Version of JIRA                                              |
| `jira_home`           | Path to the JIRA home directory                               |
| `java_home`           | Path to the JRE or JDK home directory                         |
| `jdkname`             | Name of the JDK package                                       |
| `wildcardpath`        | Path to the SSL certificate keystore file                    |
| `wildcardpass`        | Password for the SSL certificate keystore                     |
| `domain`              | Domain for JIRA (e.g., example.com)                            |
| `baseurl`             | Base URL for JIRA (e.g., jira.example.com)                     |

### Linux Cert Config Vars

| Variable                | Description                                                                                                     |
|------------------------|-----------------------------------------------------------------------------------------------------------------|
| `target_application`    | The target application name (e.g., gitlab, jira, confluence, app_name)                                          |
| `custom_certpath`      | (Optional) The custom directory to store the generated certificates                                             |
| `ad_username`          | Active Directory service account username                                                                      |
| `ad_password`          | Active Directory service account password                                                                      |
| `pki_ca_host`          | The PKI CA hostname                                                                                            |
| `pki_template_type`    | The certificate template type (e.g., example.com Webserver v1)                                                   |
| `domain`               | The target domain name                                                                                          |
| `csr_common_name`      | The common name for the CSR (e.g., gitlab.example.com)                                                          |
| `csr_email_address`    | The email address to use for the CSR                                                                           |
| `csr_country`          | The country code for the CSR (e.g., US)                                                                         |
| `csr_state`            | The state code for the CSR (e.g., GA)                                                                           |
| `csr_location`         | The location code for the CSR (e.g., Marietta)                                                                  |
| `csr_organization`     | The organization name for the CSR                                                                              |
| `csr_ou`               | The organizational unit for the CSR                                                                            |
| `san_names`            | A list of Subject Alternative Names (SANs) to include in the CSR (e.g., gitlab.example.com, 192.168.1.100)    |

## Notes
1. This playbook is designed to work with Active Directory and a Windows-based PKI server.
2. The `custom_certpath` variable can be used to specify a custom directory to store the generated certificates. If left undefined, the certificates will be stored in the default location. This will break the Jira-Certconfig playbook as it pulls from `custom_certpath`.
3. The `san_names` variable allows you to specify additional Subject Alternative Names (SANs) for the CSR.
4. The `ad_username` and `ad_password` variables should be set to the credentials of an account with the required permissions to manage certificates on the PKI server.
5. The playbook uses `curl` to interact with the PKI server, which requires the `--ntlm` flag for authentication.