# Setup DOX CLI
Install and configure **DOX CLI** on GitHub Actions runners.  
After this step, `dox` is available for all workflow steps.
More details: [DOX CLI GitHub](https://github.com/dopxlab/dox-cli)

---

## 📖 What is DOX CLI?

**DOX** is a universal tool installer and environment manager that simplifies DevOps workflows. Instead of manually installing and configuring multiple tools (terraform, kubectl, aws-cli, etc.) across different environments, DOX automates the entire process with a single command.

### Why use DOX?

✅ **Consistency** - Ensure the same tool versions across all environments  
✅ **Speed** - Install multiple tools with one command or config file  
✅ **Simplicity** - No more writing complex installation scripts  
✅ **Caching** - Download once, reuse across builds  
✅ **Declarative** - Define your toolchain in YAML  
✅ **Version Control** - Pin specific versions or use latest

Perfect for CI/CD pipelines, development environments, and containerized workflows.

---

## 🔧 Example 1: Command-based configuration
```yaml
- name: Setup DOX CLI
  uses: dopxlab/setup-dox-cli@v1
- name: Configure Go
  run: dox config go
- name: Check Go version
  run: go version
```

**Note:** You can also install multiple tools at once:
```bash
dox config jdk python terraform
```

---

## 🔧 Example 2: File-based configuration with versions
`tools.yaml`:
```yaml
az: 2.55.0          # Specific version
terraform: 1.6.0    # Specific version
kubectl:            # Latest version (no version specified)
helm: 3.13.0        # Specific version
```

Workflow:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Setup DOX CLI
        uses: dopxlab/setup-dox-cli@v1
      - name: Configure tools
        run: dox config -f tools.yaml
      - name: Verify tools
        run: |
          python3 --version
          az version
          terraform --version
          kubectl version --client
```

---

## 🔧 Example 3: Custom cache directories
```yaml
- name: Setup DOX CLI with custom paths
  uses: dopxlab/setup-dox-cli@v1
  with:
    custom-dir: "$HOME/.dox/customize"
    resources-dir: "$HOME/dox_resources"
- name: Configure tools
  run: dox config terraform kubectl
```

---

## 🔧 Example 4: Persistent cache with GitHub Actions Cache
```yaml
- name: Cache DOX resources
  uses: actions/cache@v3
  with:
    path: ~/dox_resources
    key: dox-resources-${{ runner.os }}-${{ hashFiles('**/tools.yaml') }}
    restore-keys: |
      dox-resources-${{ runner.os }}-

- name: Setup DOX CLI
  uses: dopxlab/setup-dox-cli@v1
  with:
    resources-dir: "$HOME/dox_resources"

- name: Configure tools
  run: dox config -f tools.yaml
```

---

## ⚙️ Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `custom-dir` | Custom directory for DOX customization files | No | `$HOME/.dox/customize` |
| `resources-dir` | Directory for DOX resources cache (downloaded tools/binaries) | No | `$HOME/dox_resources` |

**Note:** For Kubernetes deployments, you can use a persistent volume (RWX) and bind it to the `resources-dir` path.

---

## 📝 Version Management

In `tools.yaml`, you can specify tool versions in two ways:

```yaml
# Specific version
terraform: 1.6.0

# Latest version (leave empty or omit version)
kubectl:
```

**If no version is specified, DOX will automatically install the latest available version.**

For command-based configuration, DOX installs the latest version by default:
```bash
dox config go           # Installs latest Go
dox config jdk python   # Installs latest JDK and Python
```

---

## ✨ Features
* Installs the latest DOX CLI
* Makes `dox` available in workflows
* Supports file-based and command-based configuration
* **Install single or multiple tools in one command**
* **Version pinning or automatic latest version installation**
* **Configurable cache directories** for customization and resources
* **GitHub Actions cache integration** for faster builds
* Works on GitHub-hosted Linux runners

---

## 📄 License
MIT License