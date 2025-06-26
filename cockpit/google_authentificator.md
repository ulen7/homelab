# How to enable Google Authenticator to Cockpit for Ubuntu

A method to setup MFA in cockpit to get more security

## Install google authenticator

Install the library for google authenticator

```
sudo apt-get install libpam-google-authenticator libqrencode-dev
```
---

Creates a qr code that you can scan with your mobile, make sure to save the code somewhere.

```
google-authenticator -t -d -f -r 3 -R 30 -W -Q UTF8
```
---

Append the cockpit auth requirement to the login screen

```
sudo bash -c 'echo "auth required pam_google_authenticator.so nullok" >> /etc/pam.d/cockpit'
```

Now that everything is setup, restart cockpit

```
sudo systemctl restart cockpit
```

If you want to remove the MFA request from the login

Go into `/etc/pam.d/cockpit` and delete the line `auth required pam_google_authenticator.so nullok`

Login and see if it works.

## Troubleshoot

If the time of your server and your phone is no correct, the server may not recognize the MFA code.

## Source

- https://www.reddit.com/r/Ubuntu/comments/1cjkv3j/guide_how_to_enable_google_authenticator_to/
- https://ubuntu.com/tutorials/configure-ssh-2fa#1-overview
