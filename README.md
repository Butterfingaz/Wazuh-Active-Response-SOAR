# Wazuh-Active-Response-SOAR
Automated Endpoint Containment & Security Orchestration with Wazuh Active Response

## 1. Project Overview
This project demonstrates **Security Orchestration, Automation, and Response (SOAR)** capabilities using **Wazuh Active Response**. To minimize Mean Time to Contain (MTTC) during active brute-force or unauthorized API access attacks, a custom Bash containment script (`block-malicious-ip.sh`) was integrated with Wazuh Manager rules (`ossec_ar_snippet.xml`) to dynamically isolate malicious IPs at the host firewall level.

## 2. Active Response Configuration (`ossec_ar_snippet.xml`)
The manager configuration binds the custom containment script to high-severity Rule `100105` with an automated 10-minute (`600` seconds) quarantine timeout:

```xml
<command>
  <name>custom-ip-block</name>
  <executable>block-malicious-ip.sh</executable>
  <timeout_allowed>yes</timeout_allowed>
</command>

<active-response>
  <command>custom-ip-block</command>
  <location>local</location>
  <rules_id>100105</rules_id>
  <timeout>600</timeout>
</active-response>
```

## 3. Automated Containment Script (`block-malicious-ip.sh`)
When triggered by the Wazuh analysis engine, the endpoint agent executes the script to insert an immediate `DROP` rule into `iptables` and writes an audit trail to `active-responses.log`.

## 4. Execution & Audit Log Proof

1. <img width="915" height="169" alt="Project workspace" src="https://github.com/user-attachments/assets/e18b1eb4-bb8c-4cfe-bf03-bfe683bebf21" />

2. <img width="914" height="485" alt="Active Response Script" src="https://github.com/user-attachments/assets/d6ffff6e-75a5-4302-bff3-e3851ecdaa91" />

3. <img width="894" height="471" alt="Wazuh Manager Configuration" src="https://github.com/user-attachments/assets/aa19c4c4-3663-467d-ab6f-09fc620dde00" />

4. <img width="931" height="302" alt="Simulating the Automated Containment Execution" src="https://github.com/user-attachments/assets/2d561994-89e4-4c1a-854c-cc518da04a3b" />

5. 

https://github.com/user-attachments/assets/d6c46c84-5039-489a-b303-bd7aa1e35056



* **Containment Action:** Automated `iptables -I INPUT -s 198.51.100.99 -j DROP`
* **MITRE ATT&CK Mitigation:** `M1037` (Filter Network Traffic)
