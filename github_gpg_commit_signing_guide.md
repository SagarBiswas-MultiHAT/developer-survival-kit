<div align="center">

<a id="-the-ultimate-guide-to-github-gpg-commit-signing"></a><a id="the-ultimate-guide-to-github-gpg-commit-signing"></a>
# 🔐 The Ultimate Guide to GitHub GPG Commit Signing
### **From Zero to "Verified": Protect Your Identity, Secure Your Code, and Build Trust in Open Source**

[![GitHub Verified](https://img.shields.io/badge/GitHub-Verified_Commits-2ea44f?style=for-the-badge&logo=github)](https://docs.github.com/en/authentication/managing-commit-signature-verification)
[![GnuPG](https://img.shields.io/badge/GnuPG-2.4%2B-0093dd?style=for-the-badge&logo=gnuprivacyguard)](https://gnupg.org/)
[![Security](https://img.shields.io/badge/Security-Supply_Chain-critical?style=for-the-badge&logo=securityscorecard)](#-the-shocking-flaw-in-git-why-you-need-this)
[![PRs Welcome](https://img.shields.io/badge/PRs-Welcome-brightgreen.svg?style=for-the-badge)](./.github/CONTRIBUTING.md)

*Written in plain English so **anyone**, even a non-technical reader, can understand how digital signatures work, while giving developers an exact, copy-pasteable blueprint.*

---

[Why Care?](#-the-shocking-flaw-in-git-why-you-need-this) •
[What is GPG?](#-what-is-gpg-explained-for-everyone) •
[Step-by-Step Setup](#-step-by-step-setup-guide-zero-to-verified) •
[Pro Tips](#-pro-tips--quality-of-life-upgrades) •
[Troubleshooting](#-troubleshooting-common-errors) •
[FAQ](#-frequently-asked-questions-faq)

</div>

---

<a id="-the-shocking-flaw-in-git-why-you-need-this"></a><a id="the-shocking-flaw-in-git-why-you-need-this"></a>
## 🚨 The Shocking Flaw in Git (Why You Need This)

Did you know that **anyone on the internet can pretend to be you on GitHub** in less than 10 seconds?

By default, Git trusts whatever name and email a user types into their computer. If a stranger runs these two lines on their laptop:

```bash
git config user.name "Your Name"
git config user.email "your-public-email@example.com"
```

...and pushes malicious or broken code to a repository, **GitHub will display your profile picture and username as the author of that commit.**

> [!CAUTION]
> Without cryptographic verification, Git has **zero built-in proof** of who actually wrote a line of code. This is known as **Identity Spoofing**.

**The Solution?** **GPG Commit Signing.**  
When you sign your commits with GPG, GitHub places a green **`Verified`** badge next to your commits - proving mathematically that the code came from your machine and nobody else's.

---

## 📖 Table of Contents

1. [What is GPG? (Explained for Everyone)](#-what-is-gpg-explained-for-everyone)
2. [How GPG Works Under the Hood (Without the Math)](#-how-gpg-works-under-the-hood-without-the-math)
3. [Why GPG Matters for Your Career & Open Source](#-why-gpg-matters-for-your-career--open-source)
4. [Step-by-Step Setup Guide (Zero to Verified)](#-step-by-step-setup-guide-zero-to-verified)
   - [Step 1: Verify GPG Installation](#step-1-verify-gpg-installation)
   - [Step 2: Generate Your GPG Key Pair](#step-2-generate-your-gpg-key-pair)
   - [Step 3: Choose Your Passphrase Wisely](#step-3-choose-your-passphrase-wisely)
   - [Step 4: Find Your Key ID & Export Your Public Key](#step-4-find-your-key-id--export-your-public-key)
   - [Step 5: Add Your Public Key to GitHub](#step-5-add-your-public-key-to-github)
   - [Step 6: Configure Git to Sign Automatically](#step-6-configure-git-to-sign-automatically)
   - [Step 7: Make Your First Verified Commit](#step-7-make-your-first-verified-commit)
5. [Pro Tips & Quality-of-Life Upgrades](#-pro-tips--quality-of-life-upgrades)
6. [Security & Key Management](#-security--key-management)
7. [GPG vs. SSH Commit Signing](#-gpg-vs-ssh-commit-signing)
8. [Troubleshooting Common Errors](#-troubleshooting-common-errors)
9. [Frequently Asked Questions (FAQ)](#-frequently-asked-questions-faq)

---

<a id="-what-is-gpg-explained-for-everyone"></a><a id="what-is-gpg-explained-for-everyone"></a>
## 🧠 What is GPG? (Explained for Everyone)

**GPG** stands for **GNU Privacy Guard**. Don't let the technical name intimidate you.

Imagine living in the era of kings and queens. When a king sent an important letter across the country, how did the receiver know the letter wasn't written by an impostor?

The king poured hot wax onto the envelope and pressed his **personal signet ring** into it, creating a unique **Royal Wax Seal**.
* Only the king possessed the **ring** (it never left his finger).
* Anyone in the kingdom had a **picture of the seal** so they could inspect the wax and verify it was authentic.
* If anyone tampered with the letter on the road, the wax seal would crack and break.

**GPG is the exact same Royal Wax Seal, built for the digital world.**

---

<a id="-how-gpg-works-under-the-hood-without-the-math"></a><a id="how-gpg-works-under-the-hood-without-the-math"></a>
## 🔑 How GPG Works Under the Hood (Without the Math)

When you set up GPG, your computer creates **two linked digital keys** plus a **passphrase**:

```text
+-----------------------------------------------------------------------+
|                        YOUR GPG SECURITY TRIAD                        |
+-----------------------------------------------------------------------+
|                                                                       |
|  1. PRIVATE KEY (The King's Ring)                                     |
|     - Stays secretly locked on your computer.                         |
|     - NEVER shared with anyone (not even GitHub).                     |
|     - Used to "stamp" (sign) your commits.                            |
|                                                                       |
|  2. PUBLIC KEY (The Picture of the Seal)                              |
|     - Shared openly with the world and uploaded to GitHub.            |
|     - Cannot be used to forge your signature.                         |
|     - Used by GitHub to verify that your stamp is real.               |
|                                                                       |
|  3. PASSPHRASE (The Vault Combination)                                |
|     - A secret sentence only you know.                                |
|     - Unlocks your Private Key when you want to sign a commit.        |
|                                                                       |
+-----------------------------------------------------------------------+
```

### The Signing Flow in 3 Seconds:
1. **You write code** and run `git commit`.
2. **Your Private Key** combines your code + timestamp to create a unique mathematical stamp (Digital Signature).
3. **You push to GitHub.** GitHub uses your **Public Key** to check the stamp. If it matches and Not a single comma was altered, GitHub awards the green **`Verified`** badge!

---

<a id="-why-gpg-matters-for-your-career--open-source"></a><a id="why-gpg-matters-for-your-career--open-source"></a>
## 🌟 Why GPG Matters for Your Career & Open Source

1. **Stops Impersonators Cold:** Even if someone copies your username and email, their commits will show up as `Unverified` while yours shine with `Verified`.
2. **Proves Original Authorship (Non-Repudiation):** Your signed commit history acts as a cryptographic timestamped ledger proving when you wrote the code and that it originated from your key.
3. **Recruiter & Senior Engineer Signal:** When hiring managers or senior maintainers inspect your repositories and see `Verified` badges across your commit history, it immediately signals that you understand **Supply Chain Security** and professional engineering standards.
4. **Protects Against Tampering:** If anyone alters a single character in a signed commit, the cryptographic signature immediately invalidates.

---

<a id="-step-by-step-setup-guide-zero-to-verified"></a><a id="step-by-step-setup-guide-zero-to-verified"></a>
## 🛠️ Step-by-Step Setup Guide (Zero to Verified)

Follow these steps carefully. No prior cryptography experience is needed.

### Step 1: Verify GPG Installation

Open your terminal:
* **Windows:** Open **Git Bash** (GPG comes pre-installed with Git for Windows!).
* **macOS / Linux:** Open your default **Terminal**.

Run this command to check if GPG is ready:

```bash
gpg --version
```

<details>
<summary><b>Click here if it says "command not found"</b></summary>

* **macOS ( via Homebrew):** `brew install gnupg`
* **Ubuntu / Debian:** `sudo apt update && sudo apt install gnupg`
* **Windows:** Use **Git Bash** (included with Git) or download [Gpg4win](https://www.gpg4win.org/).
</details>

---

### Step 2: Generate Your GPG Key Pair

Start the interactive key generation wizard:

```bash
gpg --full-generate-key
```

You will be asked a few simple questions in the terminal. Here is exactly what to choose:

#### 1. Select the Key Type
* **In modern GPG (v2.4+):** You will see `(9) ECC (sign and encrypt) *default*`. Simply press **`Enter`** to choose the default. Next, press **`Enter`** again to select `(1) Curve 25519 *default*`. *(Curve 25519 is modern, blazing fast, and ultra-secure).*
* **In older GPG versions:** Choose `(1) RSA and RSA` and type `4096` when asked for the key size.

#### 2. Set the Expiration Date
```text
Key is valid for? (0)
```
* Press **`Enter`** (which selects `0`, meaning the key does not expire), or type `1y` if you prefer rotating keys yearly.
* Type **`y`** and press **`Enter`** to confirm.

#### 3. Enter Your Identity (CRITICAL STEP!)
* **Real name:** Type your full name (e.g., `Sagar Biswas`) and press **`Enter`**.
* **Email address:** Type the **exact email address** verified on your GitHub account.

> [!IMPORTANT]
> **Want to keep your personal email private?**  
> Go to **GitHub Settings → Emails**, enable *"Keep my email addresses private"*, and copy your GitHub-provided stealth email (it looks like `12345678+username@users.noreply.github.com`). Use that exact address here!

* **Comment:** Leave it blank and press **`Enter`**.
* Type **`O`** (capital letter O for Okay) and press **`Enter`**.

---

### Step 3: Choose Your Passphrase Wisely

A popup window or terminal prompt will ask you to create a **Passphrase**.

Unlike a short, complicated password (like `P@$$w0rd1`), a **Passphrase** is a memorable string of words or a sentence. Length makes it mathematically unbreakable while keeping it easy for you to type.

**Great Passphrase Examples (Do not use these exact ones!):**
* `coffee-matrix-river-planet-42` *(Random words + hyphens + number)*
* `I write clean code at 2am!` *(A memorable sentence)*
* `cloud-server-terminal-breeze-07`

> [!TIP]
> Never reuse your GitHub login password or email password as your GPG passphrase. Keep it unique and store it safely in a password manager.

---

<a id="step-4-find-your-key-id--export-your-public-key"></a><a id="step-4-find-your-key-id-export-your-public-key"></a>
### Step 4: Find Your Key ID & Export Your Public Key

Now that your key is created, run this command to view it:

```bash
gpg --list-secret-keys --keyid-format=long
```

You will see an output that looks like this:

```text
sec   ed25519/3AA5C34371567BD2 2026-10-16 [SC]
      E963579DE70CA96DE4187CB140...
uid                 [ultimate] Your Name <your-email@users.noreply.github.com>
ssb   cv25519/8D7997A54C216298 2026-10-16 [E]
```

Look at the first line starting with `sec`. The **16-character code** right after the slash `/` (in this example, `3AA5C34371567BD2`) is your **Key ID**.

Now, export your **Public Key** using your 16-character Key ID:

```bash
gpg --armor --export YOUR_KEY_ID
```

Copy the **entire output**, including the `-----BEGIN PGP PUBLIC KEY BLOCK-----` and `-----END PGP PUBLIC KEY BLOCK-----` lines:

```text
-----BEGIN PGP PUBLIC KEY BLOCK-----

mDMEaP... (your public key text here) ...
...
-----END PGP PUBLIC KEY BLOCK-----
```

---

### Step 5: Add Your Public Key to GitHub

1. Open GitHub and click your **Profile Picture** (top-right) → **Settings**.
2. In the left sidebar, click **SSH and GPG keys**.
3. Click the green **New GPG key** button next to the GPG keys section.
4. **Title:** Give it a recognizable name (e.g., `Personal Laptop` or `Windows Git Bash`).
5. **Key:** Paste the entire block you copied (including the `BEGIN` and `END` lines).
6. Click **Add GPG key** and confirm your GitHub authentication if prompted.

---

### Step 6: Configure Git to Sign Automatically

Tell your local Git which GPG key to use and enable automatic signing for all future commits:

```bash
# 1. Tell Git your GPG Key ID
git config --global user.signingkey YOUR_KEY_ID

# 2. Automatically sign every commit without needing the -S flag
git config --global commit.gpgsign true

# 3. Ensure your Git email matches your GPG email!
git config --global user.email "your-gpg-email@users.noreply.github.com"
```

Verify that auto-signing is enabled by running:

```bash
git config --global commit.gpgsign
```
*(It should print `true`).*

---

### Step 7: Make Your First Verified Commit

Go into any of your Git repositories, make a small change, and commit as usual:

```bash
git commit -m "chore: verify gpg commit signing"
```

1. A prompt will appear asking for your **GPG Passphrase**. Type it in and press Enter.
2. Check your signature locally right inside the terminal:
   ```bash
   git log --show-signature -1
   ```
   Look for the line: `gpg: Good signature from "Your Name <...>"`.
3. Push your commit to GitHub:
   ```bash
   git push
   ```

Go to your repository on GitHub and click on **Commits**. You will now see the green **`Verified`** badge next to your commit! 🎉

---

<a id="-pro-tips--quality-of-life-upgrades"></a><a id="pro-tips--quality-of-life-upgrades"></a>
## ⚡ Pro Tips & Quality-of-Life Upgrades

### 1. Stop Typing Your Passphrase on Every Single Commit
Typing your passphrase 50 times a day gets tiring. You can tell `gpg-agent` to remember your passphrase for a few hours after you type it once.

Run this in your terminal to create or update `~/.gnupg/gpg-agent.conf`:

```bash
echo "default-cache-ttl 28800" >> ~/.gnupg/gpg-agent.conf
echo "max-cache-ttl 86400" >> ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
```
* `default-cache-ttl 28800`: Caches your passphrase for **8 hours** since last use.
* `max-cache-ttl 86400`: Forces a re-entry after **24 hours** maximum.

### 2. Sign Your Git Tags & Releases Too
Want your version releases (`v1.0.0`) to also show as `Verified` on GitHub? Configure Git to sign tags automatically:

```bash
git config --global tag.gpgSign true
```

### 3. Enable Seamless Signing in VS Code
If you commit using the VS Code Source Control sidebar, open VS Code Settings (`Ctrl + ,`), search for **`git.enableCommitSigning`**, and check the box!

---

<a id="-security--key-management"></a><a id="security--key-management"></a>
## 🛡️ Security & Key Management

### What Happens If My Passphrase Leaks?
* **If ONLY your Passphrase leaks:** Nobody can impersonate you unless they *also* steal the Private Key file from your computer (`~/.gnupg`). Still, you should change your passphrase immediately using:
  ```bash
  gpg --passwd YOUR_KEY_ID
  ```
* **If BOTH your Private Key AND Passphrase leak:**
  1. Immediately go to **GitHub Settings → SSH and GPG keys**.
  2. Click **Delete** next to the compromised GPG key.
  3. Generate a brand-new key pair.

### How to Back Up Your GPG Key (Before Formatting Your PC!)
If you wipe your computer without backing up your private key, you can no longer sign commits with that key. To export a backup to a safe USB drive:

```bash
# Export Private Key (KEEP THIS FILE ULTRA-SECRET!)
gpg --armor --export-secret-keys YOUR_KEY_ID > my-private-gpg-backup.asc

# To import it on a new computer later:
gpg --import my-private-gpg-backup.asc
```

---

<a id="-gpg-vs-ssh-commit-signing"></a><a id="gpg-vs-ssh-commit-signing"></a>
## ⚖️ GPG vs. SSH Commit Signing

GitHub supports signing commits with both GPG keys and SSH keys. Which one should you choose?

| Feature | 🔐 GPG Key Signing | 🔑 SSH Key Signing |
| :--- | :--- | :--- |
| **Industry Standard** | The long-standing gold standard in Open Source | Newer alternative (introduced in Git 2.34) |
| **Expiration & Subkeys** | Rich support for subkeys, expiration & revocation | Simple key files |
| **Ecosystem Support** | Supported by GitHub, GitLab, Bitbucket, package managers | Supported by GitHub and GitLab |
| **Setup Effort** | Moderate (takes ~5 minutes) | Fast (reuses existing SSH key) |

---

<a id="-troubleshooting-common-errors"></a><a id="troubleshooting-common-errors"></a>
## 🔧 Troubleshooting Common Errors

<details>
<summary><b>1. My commit is pushed, but GitHub shows "Unverified" or no badge at all!</b></summary>

**99% of the time, this is an Email Mismatch.**
Check the email used inside your commit vs. the email attached to your GPG key:
1. Run `git log -1` and check the `Author: Name <email>` line.
2. Run `gpg --list-secret-keys` and check the `uid` email.
3. Ensure both emails match **character for character** and that the email is marked as **Verified** in your GitHub Email Settings.
</details>

<details>
<summary><b>2. Error: "gpg: signing failed: Inappropriate ioctl for device" (macOS / Linux)</b></summary>

GPG doesn't know which terminal window to display the passphrase prompt in. Fix it permanently by adding `GPG_TTY` to your shell profile:

```bash
# For Zsh (macOS default):
echo 'export GPG_TTY=$(tty)' >> ~/.zshrc && source ~/.zshrc

# For Bash:
echo 'export GPG_TTY=$(tty)' >> ~/.bashrc && source ~/.bashrc
```
</details>

<details>
<summary><b>3. Error: "error: gpg failed to sign the data" on Windows</b></summary>

Sometimes Git uses the wrong GPG binary path on Windows. Point Git directly to your GPG executable:

```bash
git config --global gpg.program "$(which gpg)"
```
</details>

---

<a id="-frequently-asked-questions-faq"></a><a id="frequently-asked-questions-faq"></a>
## ❓ Frequently Asked Questions (FAQ)

### Q: If someone clones my open-source project, deletes the `.git` folder, and uploads it as their own, does GPG stop them?
**A:** GPG cannot physically stop someone from copying public files on their own computer. **However, GPG proves you are the original creator.**
* Your original repository has a **cryptographically signed, timestamped commit history** showing step-by-step development from an earlier date.
* A thief who deletes `.git` and re-uploads the project will only have a recent timestamp and a single bulk commit.
* Combined with an open-source **LICENSE** (like MIT, Apache 2.0, or GPL), your signed commit history serves as undeniable proof when filing a **GitHub DMCA Takedown Request** (`github.com/contact/dmca`).

### Q: What happens to my old `Verified` commits if I delete my GPG key from GitHub?
**A:** If you **delete** a GPG key from GitHub, any commits signed with that key will stop showing as `Verified`. Instead of deleting an old key, let it expire or keep the public key on your profile if you haven't lost the private key to a breach.

### Q: Can I use the same GPG key on multiple computers?
**A:** Yes, by exporting and importing your secret key (see the backup section above), or you can generate a separate GPG key for each device (e.g., `Work MacBook` and `Home PC`) and add both public keys to your GitHub account!

---

## 🤝 Contributing & Sharing

Found this guide helpful? 
* ⭐ **Star this repository** to help other developers discover it!
* 🔁 **Share it** with your team or developer community to make open-source software safer for everyone.
* 🛠️ **Pull Requests** for translations, typo fixes, or additional OS tips are always welcome!

---

<div align="center">

**Authored by [Sagar Biswas (@SagarBiswas-MultiHAT)](https://github.com/SagarBiswas-MultiHAT)**  
*Part of the [Developer Second Brain & Engineering Vault](./README.md)*  
*Stay Secure. Keep Shipping. Always Sign Your Commits.*

[Back to Top](#-the-ultimate-guide-to-github-gpg-commit-signing) • [Vault Catalog (README)](./README.md) • [Contributing](./.github/CONTRIBUTING.md)

</div>

---