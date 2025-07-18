# 👤 Temporary User Account: rose (Nautilus Project)

## 📌 Overview

This document outlines the creation of a **temporary user account** for developer **Rose** as part of the Nautilus project. The access is granted on **App Server 2** (`stapp02`) in the **Stratos Datacenter** and will automatically expire on **2024-04-15**.

---

## 🧩 Problem Statement

Rose needs temporary access to `stapp02` for development work. The DevOps team must:

- Create the user `rose` (lowercase, as per policy)
- Set an expiry date: **April 15, 2024**
- Verify the account is configured correctly for automatic deactivation

---

## ✅ Solution Steps

### 1. SSH into App Server 2

```bash
ssh steve@stapp02
```

2. Create the User with Expiry
```bash
sudo useradd rose -e 2024-04-15
```
The -e flag sets the account expiration date.

```bash
sudo passwd rose
```
3. Verify User Configuration
```bash
sudo chage -l rose
```
Sample Output:
```bash
Last password change                                    : Jul 18, 2025
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Apr 15, 2024
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```