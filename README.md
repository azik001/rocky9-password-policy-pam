###  Linux Security Hardening (Rocky Linux 9)


This project demonstrates implementation of system-level password security policies on Rocky Linux 9 using PAM, faillock, and pwquality.

---

## Features

* Account lockout after 5 failed login attempts (faillock)
* Password complexity enforcement (pwquality)
* Password expiration policy (90 days)
* Password history (last 5 passwords remembered)
* Policy applied to existing users using `chage`

---

## Technologies

* PAM (Pluggable Authentication Modules)
* faillock
* pwquality
* authselect

---

## ⚙️ Configuration

### Account Lockout

* deny = 5
* unlock_time = 600 seconds
* fail_interval = 900 seconds

---

### Password Policy

* Minimum length: 12 characters
* Requires:

  * uppercase
  * lowercase
  * digit
  * special character

---

### Password Expiration

* Max age: 90 days
* Min age: 1 day
* Warning: 7 days

---

## Important Note

During testing, it was identified that on Rocky Linux 9:

> faillock.conf may not always be enforced unless parameters are explicitly defined in PAM configuration.

Example:

```
auth required pam_faillock.so preauth silent deny=5 unlock_time=600 fail_interval=900
auth [default=die] pam_faillock.so authfail deny=5 unlock_time=600 fail_interval=900
```

---

## Testing

### Check password policy

```
chage -l username
```

### Check failed attempts

```
faillock --user username
```

### Reset lock

```
faillock --user username --reset
```

---

## Lessons Learned

* SSH counts multiple password retries within a single session
* faillock counts authentication failures per session
* authselect must be used carefully
* PAM configuration may override config files

---

## Use Cases

* Linux server hardening
* Cloud VM security baseline
* DevOps environments
* CIS-like compliance preparation

---

