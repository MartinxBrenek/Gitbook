---
title: You can track your IaC sour...
---

```
You can track your IaC sources the same way you track Cisco IOS XE Software fragments. Git versions plaintext reliably. Every change is reviewable and reversible.

Terraform: Commit *.tf, modules, variables, and terraform.lock.hcl to pin providers. Do not commit state files or the .terraform working directory.

Ansible: Commit playbooks, roles, group_vars, host_vars, and inventory definitions. If you use Ansible Vault, version the encrypted files, never the vault password.

Templates and data: Commit Jinja2 templates, YAML, or CSV inventories, JavaScript Object Notation (JSON) payloads for Representational State Transfer Configuration Protocol (RESTCONF), and small Python utilities for validation.

You can also store full device configurations in Git. This helps with audit trails and comparisons.

Keep full device configuration snapshots separate from the network intent files. Use a dedicated snapshots folder or, better, a separate repository with a predictable structure like snapshots/<site>/<device>/<YYYY-MM-DD>/running-config.txt, to ensure that extensive diffs do not slow down the review process for network changes.

Redact secrets or avoid committing them. Automate redaction with a pre-commit hook.

Treat running configuration snapshots as read-only evidence. Make all network changes by editing the desired state in version-controlled IaC files, reviewed in merge requests and applied by automation. Keep device configuration or state as archived snapshots.
```
