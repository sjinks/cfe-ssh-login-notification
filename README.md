# SSH Login Notification (CFEngine module)

[![CI Pipeline](https://github.com/sjinks/cfe-ssh-login-notification/actions/workflows/ci.yml/badge.svg)](https://github.com/sjinks/cfe-ssh-login-notification/actions/workflows/ci.yml)

Configures SSH login email notifications via PAM by installing a small notifier script and ensuring PAM runs it on session open.

## What it does

- Installs a notifier script at `/usr/local/sbin/ssh-login-notifier` (template: `ssh-login-notifier.mustache`).
- Ensures PAM for SSH (`/etc/pam.d/sshd`) runs the notifier on session open.
- Sends an email with basic session details and recent authentication log entries.

## Inputs

This module expects the following inputs under the `ssh_login_notification` namespace:

- `mailfrom`: Sender email address.
- `mailto`: Recipient email address.

```json
{
  "variables": {
    "ssh_login_notification:ssh_login_notification.mailfrom": {
      "value": "sender@example.com"
    },
    "ssh_login_notification:ssh_login_notification.mailto": {
      "value": "security@example.com"
    }
  }
}
```

## Usage

Add the module to your CFEngine Build System (`cfbs`) project and provide inputs:

```sh
cfbs add https://github.com/sjinks/cfe-ssh-login-notification
```

The module copies:

- `ssh-login-notification.cf` to `services/cfbs/modules/ssh-login-notification/ssh-login-notification.cf`
- `ssh-login-notifier.mustache` to `templates/ssh-login-notification/ssh-login-notifier.mustache`

## Requirements

- PAM with `pam_exec.so` (provided by `libpam-modules` on Debian/Ubuntu).
- A `mailx` implementation (provided by `mailutils` on Debian/Ubuntu).

## Notes

- The notifier runs only on `open_session`.
- Log excerpts are taken from `/var/log/auth.log` or `/var/log/secure` when available.
