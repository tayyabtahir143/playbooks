# AWX Patching

## 1) AWX Credential
Create one Machine credential:
- Name: ansible-awx
- Username: ansible
- SSH Private Key: contents of ~/.ssh/ansible_awx
- Passphrase: empty (if created with -N "")

## 2) AWX Project
Create a Project pointing to this repo.

## 3) AWX Inventory
Create an Inventory and add hosts (or sync from repo inventory if you prefer).
Do NOT set ansible_user=root anywhere.

## 4) Job Templates
Create job templates:
- Precheck: playbooks/00-precheck.yml
- Patch: playbooks/10-patch-linux.yml
- Reboot: playbooks/20-reboot-if-needed.yml
- Summary: playbooks/90-summary.yml

Attach Credential: ansible-awx
Enable Privilege Escalation (become) if your AWX asks per-template.

## 5) Workflow (recommended)
Create a Workflow Template:
1) Precheck
2) Patch
3) Reboot
4) Summary

## Notes
- Unreachable nodes are skipped automatically.
- Patching continues even if some hosts fail a task.
- Reboot logic sets reboot_required and reboots only those nodes.

