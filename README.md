# Setup DOX CLI

Install and configure **DOX CLI** on GitHub Actions runners.  
After this step, `dox` is available for all workflow steps.

More details: [DOX CLI GitHub](https://github.com/dopxlab/dox-cli)

---

## 🔧 Example 1: File-based configuration

`tools.yaml`:

```yaml
az:
terraform:
````

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
```

---

## 🔧 Example 2: Command-based configuration

```yaml
- name: Setup DOX CLI
  uses: dopxlab/setup-dox-cli@v1

- name: Configure Go
  run: dox config go

- name: Check Go version
  run: go version
```

---

## ✨ Features

* Installs the latest DOX CLI
* Makes `dox` available in workflows
* Supports file-based and command-based configuration
* Works on GitHub-hosted Linux runners

---

## 📄 License

MIT License
