# OpenSSH Static Build

Builds a static OpenSSH binary with bundled **zlib** and **OpenSSL**, suitable for deployment on systems where upgrading packages is difficult (e.g., legacy servers).

## Requirements

- **Build tools:** `gcc`, `make`, `autoconf`, `automake`
- **Network:** `curl` (for downloading sources)

### Install dependencies

**CentOS / RHEL:**
```bash
sudo yum install autoconf automake gcc make curl
```

**Debian / Ubuntu:**
```bash
sudo apt-get install autoconf automake build-essential curl
```

## Usage

```bash
./openssh-build-static.sh
```

The script downloads sources, builds zlib and OpenSSL statically, then compiles OpenSSH linked against them. Output is installed under `/opt/openssh`.

## Build stages

1. **zlib** (1.3.2) – static build
2. **OpenSSL** (1.1.1w) – static build (`no-shared`)
3. **OpenSSH** (V_10_2_P1) – configured with bundled zlib and OpenSSL

## Output

| Path | Description |
|------|-------------|
| `/opt/openssh/` | Installed binaries (`sshd`, `ssh`, `scp`, etc.) and config |
| `/opt/openssh/var/empty/` | Privsep directory |

## Customization

Edit the script to change versions:

```bash
ZLIB_VERSION=1.3.2
OPENSSL_VERSION=1.1.1w
OPENSSH_VERSION=V_10_2_P1
```

Change `prefix` (default: `/opt/openssh`) to install elsewhere.

## Notes

- **CentOS 6:** The script applies patches to `configure.ac` to work around older `autoconf` macros.
- **Clean builds:** The script removes `root`, `build`, and `dist` at the start. Comment out line 21 to keep cached builds for debugging.
- **sshd_config:** Enables `PubkeyAuthentication`, disables other auth methods, and strips Kerberos/GSSAPI options.
