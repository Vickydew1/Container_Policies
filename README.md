🔐 KubeArmor VM/Bare-metal Container Hardening Policies
=======================================================

This repository provides a curated collection of **container-scoped runtime security policies** for **VM or bare-metal environments** using **KubeArmor**.Each policy enhances **workload protection** by enforcing runtime restrictions at the **container or host level** — without requiring Kubernetes.

⚙️ Overview
-----------

These policies defend against:

*   🛡️ Defense Evasion
    
*   🔑 Credential Exfiltration
    
*   🔍 System Reconnaissance
    
*   📦 Unauthorized Package Installation
    
*   🔐 Certificate Tampering
    
*   🎭 Masquerading & Persistence
    
*   📝 Unauthorized File Writes
    
*   🌐 Network & Remote Access Abuse
    

All policies are written in **KubeArmorPolicy (container scope)** format, deployable via the karmor CLI on **Ubuntu-based VMs**.

🧰 Prerequisites
----------------

Ensure your system meets the following requirements:

ComponentDescription**OS**Ubuntu 22.04+ (VM or Bare-metal)**Container Runtime**Docker or containerd**KubeArmor**Installed and running**LSM**AppArmor / SELinux / BPF-LSM (enabled)**CLI**karmor installed

Check your setup:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo karmor status   `

Expected output:✅ KubeArmor is active✅ LSM: AppArmor (or BPF-LSM) enabled✅ gRPC connected

🧭 Environment Setup Summary
----------------------------

Below is the summarized workflow we followed during setup:

1.  **Created Ubuntu VM in Oracle Cloud (OCI)**→ Canonical Ubuntu 22.04 image, with public key authentication
    
2.  curl -s -L https://raw.githubusercontent.com/kubearmor/KubeArmor/main/install.sh | sudo bash
    
3.  sudo systemctl status kubearmorsudo karmor status
    
4.  docker run -dit --name test-container ubuntu:latest
    
5.  **Applied and validated multiple KubeArmor container policies**
    

📋 Policy Categories
--------------------

CategoryDescription🛡️ **Defense Evasion**Detect and block attempts to disable or bypass security controls🔑 **Credential Exfiltration**Prevent data theft via scp, ftp, etc.🔍 **System Reconnaissance**Detect enumeration via whoami, id, etc.📦 **Package Management**Restrict apt-get, yum, or similar commands🔐 **Trusted Certificate Tampering**Protect /etc/ssl, /usr/local/share/ca-certificates/🎭 **Masquerading & Persistence**Block unauthorized file writes or privilege escalation📝 **File Integrity Monitoring**Audit or block writes under /etc, /dev, /dev/shm🌐 **Remote Services**Monitor SSH, Sudoers, or /etc/hosts.allow activity

🚀 Applying Policies
--------------------

### 1️⃣ Create a policy file

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   nano .yaml   `

### 2️⃣ Apply the policy

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo karmor vm policy add .yaml   `

### 3️⃣ Optional: Apply via specific gRPC endpoint

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo karmor vm policy add .yaml --gRPC 127.0.0.1:50051   `

🧪 Testing Policies
-------------------

Example — testing a file write protection policy:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   # Access the container  sudo docker exec -it test-container bash  # Attempt restricted operation  touch /dev/shm/testfile  # Should trigger Audit/Block  # Exit container  exit   `

Monitor logs:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo karmor logs   `

Expected message:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   [Audit] Detected attempt to write to /dev/shm folder   `

🗑️ Removing Policies
---------------------

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo karmor vm policy delete .yaml   `

🧠 Example Workflow (End-to-End)
--------------------------------

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   # 1. Create policy  vim write-under-dev-dir.yaml  # 2. Apply  sudo karmor vm policy add write-under-dev-dir.yaml  # 3. Verify in logs  sudo karmor logs  # 4. Test in container  docker exec -it test-container bash  touch /dev/testfile  # 5. Remove when done  sudo karmor vm policy delete write-under-dev-dir.yaml   `

🧾 Policy Library (Summary)
---------------------------

Policy NameDescriptionActionpkg-mngr-exec.yamlPrevent package installations (apt-get, dpkg)Blockprevent-kubectl-cp.yamlBlock tar execution (used by kubectl cp)Blockremote-file-copy.yamlBlock scp, rsync, ftp, sftpBlockremote-services.yamlAudit SSH and sudoers modificationsAuditsystem-owner-discovery.yamlDetect user enumeration commandsAudittrusted-cert-mod.yamlMonitor changes to certificate storesAuditwrite-etc-dir.yamlDetect file creation under /etc/Auditwrite-in-shm-dir.yamlAudit writes to /dev/shm/Auditwrite-under-dev-dir.yamlAudit file writes in /dev/Audit

🐞 Troubleshooting Guide
------------------------

### Policy not taking effect

*   Run sudo karmor status
    
*   Ensure container is **running** and **KubeArmor service** is active
    
*   Check syntax and indentation in YAML
    

### Operations not blocked

*   Ensure policy was applied successfully
    
*   Paths in rules must match container paths
    
*   Check logs: sudo karmor logs
    

### gRPC errors

*   Default address: 127.0.0.1:50051
    
*   Verify service: sudo systemctl status kubearmor
    
*   View logs: journalctl -u kubearmor -f
    

📚 References
-------------

*   [KubeArmor Official Docs](https://docs.kubearmor.io/)
    
*   [KubeArmor Policy Examples](https://github.com/kubearmor/KubeArmor/tree/main/examples)
    
*   [AccuKnox Platform](https://app.demo.accuknox.com/)
    
*   [VM/Bare-metal Deployment Guide](https://docs.kubearmor.io/kubearmor/quick-links/deployment_guide)
    

**⭐ Star this repo if you find it useful!****🛠 Maintained by:** Vicky Dewangan Forward Deployed Engineer