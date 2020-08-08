# Managing Secrets

The purpose of this document is to provide contributors information on how to obtain access to Toronto Community Network assets.

# Introduction
The Toronto Community Network uses [Bitwarden](https://bitwarden.com/) to share and manage secrets across the organization.

Bitwarden is a free and open-source password management service that stores sensitive information such as website credentials in an encrypted vault.

Bitwarden supports the following clients:

- Desktop (Linux/MacOS/Windows)
- Web Browser (Chrome/Safari/Firefox/Vivaldi/Opera/Brave/Edge/Tor Browser)
- Mobile (Android/iPhone)
- Command Line (via NPM)

For more information about supported clients, see Bitwarden's [Download options](https://bitwarden.com/download/).


# Accessing Secrets
The Toronto Community Network is using the self-hosted version of Bitwarden

The URL can be accessed at [https://tomesh.infectedroot.com](https://tomesh.infectedroot.com).

For secrets to be shared, they must exist within the [Toronto Community Network organization](https://tomesh.infectedroot.com/#/organizations/e4573938-547f-4157-a977-b28fc15bcec0/vault).

When using the Bitwarden applications, please make sure to [change the Server URL](https://bitwarden.com/help/article/change-client-environment/#:~:text=On%20the%20home%20screen%20of,com%20for%20the%20Server%20URL) in the Bitwarden settings to the URL above. 

# Obtaining Access
To obtain access to Bitwarden, it is a 4-step process.

1. Send an email to operations@tomesh.net with the following information
	- Your Name
	- GitHub Handle
	- Detailed descripition of your use case or requirement

2. A member of our Project Operations team will invite you to Bitwarden.
	- Bitwarden will send you an email from tomeshnet@gmail.com
	- Please make sure to whitelist the email address
  
3. Use the link in the invitation to create an account

4. After registration, Bitwarden requires us to confirm your account. Upon confirmation, you will have access to the organization

We recommend that you setup [two-factor authentication](https://bitwarden.com/help/article/setup-two-step-login/) on your account to increase the security of your account. 


# Access Control
When storing credentials, you must store them within the [Toronto Community Network organization](https://tomesh.infectedroot.com/#/organizations/e4573938-547f-4157-a977-b28fc15bcec0/vault).

Bitwarden organizes secrets in groups called [Collections](https://bitwarden.com/help/article/collections/).

Within those [Collections](https://bitwarden.com/help/article/collections/) are four [User Types]( https://bitwarden.com/help/article/user-types-access-control/#user-types): User, Manager, Admin, and Owner.

In order to maintain integrity of the Toronto Community Network operations, most people will only need User or Manager access.

The Project Operations team will determine your access based on your use case or requirements.


# Storing Secrets
All secrets must be attached to a [Collection](https://bitwarden.com/help/article/collections/) to faciliate access control. Please be sure to attach the secret to an existing [Toronto Community Network Work Group](https://github.com/orgs/tomeshnet/teams/toronto-community-network/teams). These collection names start with "_OU".

Additional Collections may be created or utilized to classify the secret such as _Social, Service Accounts, Website, and more_.

When naming your credential, please use the following convention: _System/Function - Identifer_.

> **For example:**
> - A public SSH key may be named _"Public Key - Contributor A"_
> - A GitHub deployment key may be named _"GitHub - Deploy Key (docs.tomesh.net)"_
> - A shared login social media may be named _"Twitter - tomeshnet"_

When determining a password or passphrase, please use strong entropy to prevent unauthoized access. For more information, please see our [Recommended Best Practises](#Recommended-Best-Practises) below.

All public keys and private keys must be saved as _Secured Notes_.

Please be mindful that secrets stored in this system can be accessed by any authorized contributor in the Toronto Community Network. While your connection to the system is secured via SSL, the infrastructure it is hosted on is not PCI-DSS compliant. Please refrain from using the _Card_ and _Identity_ item types for storing any personal information.

For more information about managing items, please see Bitwarden's offical documentation on [Managing Items](https://bitwarden.com/help/article/managing-items/).


# Data Backup
The VM that hosts the Bitwarden container is backed up on a weekly basis. 

Administrators of the [Toronto Community Network organization](https://tomesh.infectedroot.com/#/organizations/e4573938-547f-4157-a977-b28fc15bcec0/vault) have the ability to export the entire vault to a JSON or CSV file.

As the export function provides secrets in clear-text, please secure or encrypt the file immediately.


# Recommended Best Practises
The Government of Canada's [Canadian Centre for Cyber Security](https://cyber.gc.ca/) provides recommendations for organizations to protect networks, systems, and information. By using these best practices, you can protect yourself and the integrity of the Toronto Community Network.

- [Password Managers-Security (ITSAP.30.025)](https://cyber.gc.ca/en/guidance/password-managers-security-itsap30025)

- [Best Practices for Passphrases and Passwords (ITSAP.30.032)](https://www.cyber.gc.ca/en/guidance/best-practices-passphrases-and-passwords-itsap30032)
