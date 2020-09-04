# Managing Secrets

The purpose of this document is to provide information to contributors on how to store and view the Toronto Community Network's "secrets", such as passwords, keys, credentials, and other information kept private for security reasons.

## Introduction
The Toronto Community Network uses [Bitwarden](https://bitwarden.com/) to share and manage secrets across the organization. Bitwarden is a free and open-source password management service that stores sensitive information (such as website credentials) in an encrypted vault.

Bitwarden supports the following clients:

- Desktop (Linux/macOS/Windows)
- Web Browser (Chrome/Safari/Firefox/Vivaldi/Opera/Brave/Edge/Tor Browser)
- Mobile (Android/iPhone)
- Command Line (via NPM)

For more information about supported clients, see Bitwarden's [download options](https://bitwarden.com/download/).


## Accessing Secrets
The Toronto Community Network is using the self-hosted version of Bitwarden, and can be accessed at [https://pass.tomesh.net](https://pass.tomesh.net).

For secrets to be shared, they must exist within the [Toronto Community Network organization](https://pass.tomesh.net/#/organizations/e4573938-547f-4157-a977-b28fc15bcec0/vault).

When using the Bitwarden applications, please make sure to [change the Server URL](https://bitwarden.com/help/article/change-client-environment/#:~:text=On%20the%20home%20screen%20of,com%20for%20the%20Server%20URL) in the Bitwarden settings to the `https://pass.tomesh.net`.  

## Obtaining Access
Obtaining access to Bitwarden is a 4-step process.
 
1.  Send an email to operations@tomesh.net with the following information:
	- Your Name
	- GitHub Handle
	- Detailed description of your use case or requirement.

2.  A member of our Project Operations team will invite you to Bitwarden.
	- Bitwarden will send you an email from tomeshnet@gmail.com
	- Please make sure to whitelist the email address.
  
3.  Use the link in the invitation to create an account.

4.  After registration, Bitwarden requires us to confirm your account. Upon confirmation, you will have access to the organization.

We recommend that you setup [two-factor authentication](https://bitwarden.com/help/article/setup-two-step-login/) on your account to increase the security of your account. 


## Access Control
When storing credentials, you must store them within the [Toronto Community Network organization](https://pass.tomesh.net/#/organizations/e4573938-547f-4157-a977-b28fc15bcec0/vault). Bitwarden organizes secrets in groups called [Collections](https://bitwarden.com/help/article/collections/) and within those Collections are four [User Types]( https://bitwarden.com/help/article/user-types-access-control/#user-types): _User_, _Manager_, _Admin_, and _Owner_.

In order to maintain integrity of the Toronto Community Network operations, most users will be granted only User or Manager access. The Project Operations team will determine your access based on your use case or requirements.


## Storing Secrets
All secrets must be attached to a [Collection](https://bitwarden.com/help/article/collections/) to facilitate access control. Please be sure to attach the secret to an existing [Toronto Community Network Work Group](https://github.com/orgs/tomeshnet/teams/toronto-community-network/teams). These collection names start with `_OU`.

Additional Collections may be created or utilized to classify the secret such as _Social_, _Service_ _Accounts_, _Website_, and more.

When naming your credential, please use the following convention: _System/Function - Identifier_.

> **For example:**
> - A public SSH key may be named _"Public Key - Contributor A"_
> - A GitHub deployment key may be named _"GitHub - Deploy Key (docs.tomesh.net)"_
> - A shared login social media may be named _"Twitter - tomeshnet"_

When determining a password or passphrase, please use strong entropy to prevent unauthorized access. For more information, please see our [Recommended Best Practices](#recommended-best-practices) below.

All public keys and private keys must be saved as _Secured Notes_.

Please be mindful that secrets stored in this system can be accessed by any authorized contributor in the Toronto Community Network. While your connection to the system is secured via TLS, the secrets you choose to store in the group can be seen by others. Please refrain from using the system for storing any personal information.

For more information about managing items, please see Bitwarden's official documentation on [Managing Items](https://bitwarden.com/help/article/managing-items/).


## Data Backup
The VM that hosts the Bitwarden container is backed up on a weekly basis. 

Administrators of the [Toronto Community Network organization](https://pass.tomesh.net/#/organizations/e4573938-547f-4157-a977-b28fc15bcec0/vault) have the ability to export the entire vault to a JSON or CSV file.

As the export function provides secrets in clear-text, please secure or encrypt the file immediately.


## Recommended Best Practices
The Government of Canada's [Canadian Centre for Cyber Security](https://cyber.gc.ca/) provides recommendations for organizations to protect networks, systems, and information. By using these best practices, you can protect yourself and the integrity of the Toronto Community Network.

- [Password Managers-Security (ITSAP.30.025)](https://cyber.gc.ca/en/guidance/password-managers-security-itsap30025)
- [Best Practices for Passphrases and Passwords (ITSAP.30.032)](https://www.cyber.gc.ca/en/guidance/best-practices-passphrases-and-passwords-itsap30032)
