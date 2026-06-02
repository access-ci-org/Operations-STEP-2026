# Troubleshooting: nvm & Node.js Setup on Ubuntu 24.04 (Jetstream2)

Common issues encountered during the environment setup steps and how to resolve them.

---

## nvm Installation Issues

### Ran `curl` with `sudo`

If `sudo` was used, nvm installs into `/root/.nvm` instead of the current user's home directory, and the shell profile edits go to root's `~/.bashrc`.

**Fix:** Remove the bad install and retry without `sudo`:

```bash
sudo rm -rf /root/.nvm
rm -rf ~/.nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
nvm --version
```

---

### `nvm: command not found` after install

The installer appends an init block to `~/.bashrc`, but login shells (e.g. SSH sessions) may source `~/.bash_profile` or `~/.profile` instead.

**Fix:** Reload the shell profile manually:

```bash
source ~/.bashrc
```

If that doesn't work, check which profile files received the nvm block:

```bash
grep -n 'nvm' ~/.bashrc ~/.bash_profile ~/.profile 2>/dev/null
```

If none of them have it, the installer may have targeted a different file. Manually add the following block to `~/.bashrc`:

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

Then run `source ~/.bashrc` again.

---

### Checksum / corrupted download error during nvm install

A network hiccup can cause a partial download, which fails checksum verification.

**Fix:** Remove and retry the install:

```bash
rm -rf ~/.nvm
# Also remove the nvm block from ~/.bashrc before retrying
source ~/.bashrc
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
nvm --version
```

To inspect the downloaded script before running it:

```bash
curl -o install_nvm.sh https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh
head -5 install_nvm.sh   # should look like a shell script, not an HTML error page
bash install_nvm.sh
```

---

## Node.js Installation Issues (`nvm install node`)

### Checksum error during `nvm install node`

nvm verifies the downloaded Node.js tarball before extracting. A partial download causes this to fail.

**Fix:** Clear the nvm download cache and retry:

```bash
rm -rf ~/.nvm/.cache
nvm install node
```

> Note: Clearing `~/.nvm/.cache` only removes cached tarballs. Any already-installed Node versions in `~/.nvm/versions/` are unaffected.

If the error persists, you can skip checksum verification as a last resort (acceptable in a workshop environment):

```bash
NVM_NO_HASH=1 nvm install node
```

---

## Completely Removing nvm and Node

To start completely fresh:

```bash
# 1. Remove everything nvm manages (nvm itself, all Node versions, cache)
rm -rf ~/.nvm

# 2. Remove the nvm init block from your shell profile
nano ~/.bashrc
# Delete the block that looks like:
#   export NVM_DIR="$HOME/.nvm"
#   [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
#   [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

# Also check these files if the block was added there instead:
# ~/.bash_profile
# ~/.profile

# 3. Reload the shell
source ~/.bashrc

# 4. Confirm nvm is gone
nvm --version   # should return "command not found"
```

After this, follow the original setup steps to reinstall nvm and Node.

---

## Quick Diagnostics

Run these to identify the likely cause of an issue:

```bash
echo $SHELL                          # should be /bin/bash
ls -la ~/.nvm                        # check ownership — should NOT be root
grep -c 'nvm' ~/.bashrc              # prints count of nvm lines; should be 3 after a successful install
nvm ls                               # lists installed Node versions; should show a version number (e.g. v22.3.0) after a successful install
```
