# 🔍 Bug Bounty Recon Methodology

> **Target:** `mydukaan.io`
> ⚠️ Only perform reconnaissance and security testing on targets that are explicitly authorized and within scope.

---

# 🌐 Bug Bounty & Vulnerability Disclosure Programs

### Google Dork

```text
site:*.org intext:"bug bounty"
```

### Programs

* Open Bug Bounty
* Decred Bug Bounty
* World Bank Vulnerability Disclosure Program

---

# 📁 Create Target Directory

```bash
mkdir targets
cd targets
ls
```

Target:

```text
https://mydukaan.io/
```

---

# 🖥️ System Information

Check your Linux distribution:

```bash
cat /etc/os-release
```

Example output:

```text
PRETTY_NAME="Ubuntu 24.04.4 LTS"
```

Check Cargo:

```bash
cargo --version
```

Example output:

```text
cargo 1.86.0 (adf9b6ad1 2025-02-28)
```

---

# 1️⃣ Subdomain Enumeration

Subdomain enumeration helps discover potential assets associated with an authorized target.

---

## 🔎 Amass

### Installation

```bash
sudo snap install amass
```

### Passive Enumeration

```bash
amass enum -passive -d mydukaan.io -o amass.txt
```

### Output

```text
amass.txt
```

---

## 🔎 Subfinder

### Check Go Installation

```bash
go version
```

### Install Subfinder

```bash
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
```

### Add Go Binary Directory to PATH

```bash
echo 'export PATH=$PATH:$HOME/go/bin' >> ~/.bashrc
source ~/.bashrc
```

### Verify Installation

```bash
subfinder -version
```

### Check Installed Go Tools

```bash
ls ~/go/bin
```

### Run Subfinder

```bash
subfinder -d mydukaan.io --all -recursive -o subfinder_output.txt
```

### Output

```text
subfinder_output.txt
```

---

## 🔎 Assetfinder

### Install Assetfinder

```bash
go install github.com/tomnomnom/assetfinder@latest
```

### Reload PATH

```bash
source ~/.bashrc
```

### Check Installation

```bash
ls ~/go/bin/
```

### Run Assetfinder

```bash
assetfinder -subs-only mydukaan.io | tee assetfinder.txt
```

### Output

```text
assetfinder.txt
```

---

## 🔐 TLSX

TLSX can be used to gather TLS/SSL information from hosts.

### Install TLSX

```bash
go install github.com/projectdiscovery/tlsx/cmd/tlsx@latest
```

### Reload PATH

```bash
source ~/.bashrc
```

### Verify Installation

```bash
tlsx -version
```

---

## 📜 Certificate Transparency Logs (crt.sh)

Certificate Transparency logs can reveal subdomains associated with certificates.

### Install jq

```bash
sudo apt update
sudo apt install jq -y
```

### Enumerate Subdomains

```bash
curl -s 'https://crt.sh/?q=%.mydukaan.io&output=json' \
| jq -r '.[].name_value' \
| sed 's/\*\.//g' \
| sort -u \
| tee crt.txt
```

### Output

```text
crt.txt
```

---

## 🔎 Findomain

### Install Dependencies

```bash
sudo apt update
sudo apt install -y curl unzip
```

### Download Findomain

```bash
curl -LO https://github.com/findomain/findomain/releases/latest/download/findomain-linux.zip
```

### Extract

```bash
unzip findomain-linux.zip
```

### Make Executable

```bash
chmod +x findomain
```

### Move to System PATH

```bash
sudo mv findomain /usr/local/bin/findomain
```

### Verify Installation

```bash
findomain --version
```

### Run Findomain

```bash
findomain --quiet -t mydukaan.io | tee findomain.txt
```

### Output

```text
findomain.txt
```

---

# 📂 Expected Output Files

After running the enumeration tools, your directory may contain:

```text
targets/
│
├── amass.txt
├── subfinder_output.txt
├── assetfinder.txt
├── crt.txt
└── findomain.txt
```

---

# 🛠️ Useful PATH Configuration

To ensure both Go and Cargo tools work correctly:

```bash
echo 'export PATH="$HOME/go/bin:$HOME/.cargo/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Verify:

```bash
echo $PATH
```

---

# 📊 Subdomain Enumeration Summary

| Tool        | Purpose                              | Output File            |
| ----------- | ------------------------------------ | ---------------------- |
| Amass       | Passive subdomain enumeration        | `amass.txt`            |
| Subfinder   | Fast passive subdomain discovery     | `subfinder_output.txt` |
| Assetfinder | Find related domains and subdomains  | `assetfinder.txt`      |
| crt.sh      | Certificate Transparency enumeration | `crt.txt`              |
| Findomain   | Multi-source subdomain discovery     | `findomain.txt`        |
| TLSX        | TLS/SSL information gathering        | N/A                    |

---

# ⚠️ Important Notes

* Always check the bug bounty program's scope before testing.
* Only test assets that are explicitly authorized.
* Passive subdomain enumeration is only the beginning of reconnaissance.
* Keep results from each tool in separate files.
* Later, duplicate results can be combined and cleaned for further analysis.

---

# 🚀 Next Step

After collecting results from all enumeration tools, combine and remove duplicate subdomains:

```bash
cat amass.txt subfinder_output.txt assetfinder.txt crt.txt findomain.txt \
| sort -u \
| tee all_subdomains.txt
```

Check the number of unique subdomains:

```bash
wc -l all_subdomains.txt
```

---

**Happy Learning & Happy Hunting! 🐞🔍**
