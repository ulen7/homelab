# Enabling Google Authenticator for Cockpit on Ubuntu

This guide explains how to enable multi-factor authentication (MFA) in Cockpit using Google Authenticator, adding an additional layer of security.

## Install Required Packages

Install the Google Authenticator PAM module and QR code library:

```
sudo apt-get install libpam-google-authenticator libqrencode-dev
```
---
## Configure Google Authenticator

Run the following command to generate your secret key and display a QR code. Scan this QR code using the Google Authenticator app on your mobile device. Be sure to save the secret key in a safe place in case you need to reconfigure the app.

```
google-authenticator -t -d -f -r 3 -R 30 -W -Q UTF8
```
---

## Update Cockpit PAM Configuration

To require MFA when logging in to Cockpit, append the following line to the PAM configuration file:

```
sudo bash -c 'echo "auth required pam_google_authenticator.so nullok" >> /etc/pam.d/cockpit'
```
> The nullok option allows users without a Google Authenticator configuration to log in normally. Remove it if you want to enforce MFA for all users.
---
## Restart Cockpit

Apply the changes by restarting the Cockpit service:

```
sudo systemctl restart cockpit
```
---
## Optional: Disable MFA for Cockpit

To disable MFA, simply remove the line added earlier from the PAM configuration file:
1. Open `/etc/pam.d/cockpit` in a text editor.
2. Delete the following line:
> `auth required pam_google_authenticator.so nullok`
3. Restart cockpit
> sudo systemctl restart cockpit
4. Login and see if it works.
---
## Troubleshooting
- Time Sync Issues: Ensure the system time on your server matches the time on your mobile device. Use timedatectl to verify and configure time synchronization.
- Access Locked: If you misconfigure MFA and are locked out, you can regain access by editing the PAM configuration from a recovery shell or via SSH (if not affected).

## Source

- [Ubuntu SSH 2FA Tutorial](/https://ubuntu.com/tutorials/configure-ssh-2fa#1-overview)
- [Reddit](/https://www.reddit.com/r/Ubuntu/comments/1cjkv3j/guide_how_to_enable_google_authenticator_to/)
