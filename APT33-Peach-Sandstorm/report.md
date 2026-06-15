# APT33 / Peach Sandstorm — Threat Intelligence Report

**TLP:** WHITE  
**Campaign Period:** February – July 2023  
**Author:** Adham Ayman  
**Confidence Level:** High  
**Report ID:** CTI-2023-IR-001  

---

## 1. Executive Summary

Between February and July 2023, the Iranian nation-state threat actor **APT33** (also known as Peach Sandstorm, formerly Holmium) conducted a large-scale **password spray campaign** targeting thousands of organizations worldwide. The actor focused on three strategic sectors: **defense, aerospace, and pharmaceuticals** — all directly tied to Iranian national interests.

After gaining initial access, the group used a combination of open-source red team tools and custom malware to move through victim environments, establish persistence, and in limited cases, exfiltrate sensitive data. No destructive tools were deployed, confirming this campaign was driven by **intelligence collection**, not sabotage.

The timing aligns with the collapse of JCPOA nuclear negotiations and tightening international sanctions, which pushed Iran toward cyber espionage as a substitute for information it could no longer obtain through legal or diplomatic channels.

---

## 2. Diamond Model

### 🔴 Adversary
| Field | Detail |
|-------|--------|
| Name | APT33 / Peach Sandstorm |
| Aliases | Holmium · Elfin · Refined Kitten |
| Origin | Iran |
| Sponsor | Iranian government (IRGC-linked) |
| Type | Nation-state threat actor |

> Source: Microsoft Threat Intelligence — Sep 2023

---

### 🟣 Capability
**Initial Access:**
- Large-scale password spray attacks against thousands of organizations
- Exploitation of **CVE-2022-26134** (Atlassian Confluence RCE)
- Exploitation of **CVE-2022-47966** (Zoho ManageEngine RCE)

**Post-Compromise Tools:**
- `AzureHound` — Azure environment reconnaissance
- `ROADtools` — Cloud data access and enumeration
- `Golang binary` — Custom reconnaissance tool
- `AnyDesk` — Abused as a remote management tool for persistence
- `AD Explorer` — Active Directory enumeration
- `Azure Arc` — Used to establish persistence via attacker-controlled Azure subscription
- Lateral movement via **SMB**
- `Golden SAML` — Credential forging for cloud access

> Source: Microsoft Threat Intelligence — Sep 2023 · Certfa Radar — Sep 2023

**Attack Chain:**
```
Password Spray → Exploitation → Discovery → Persistence → Lateral Movement → (Limited) Exfiltration
```

---

### 🟡 Infrastructure
> ⚠️ IPs sourced from Certfa Radar threat feed. Confidence: **Low-Medium**. All values are defanged. Verify before operational use.

| Indicator | Type | Source | Confidence |
|-----------|------|--------|------------|
| `102[.]129.215.40` | IP Address | Certfa Radar | Low-Med |
| `108[.]62.118.240` | IP Address | Certfa Radar | Low-Med |
| `192[.]52.166.76` | IP Address | Certfa Radar | Low-Med |
| `76[.]8.60.64` | IP Address | Certfa Radar | Low-Med |

> Source: [Certfa Radar — Peach Sandstorm Threat Entry](https://radar.certfa.com/en/threats/view/3dd41ae0/)

---

### 🔵 Victim
| Field | Detail |
|-------|--------|
| Sectors | Defense · Aerospace · Pharmaceuticals |
| Geography | Middle East · United States · Europe |
| Target Profile | High-value strategic organizations |
| Scale | Thousands of orgs sprayed — selective follow-up on successful access |

> Source: Microsoft Threat Intelligence — Sep 2023 · Certfa Radar — Sep 2023

---

### 🟢 Socio-Political Context
- **Motive:** Intelligence collection in support of Iranian state interests
- **Intent:** Espionage — no destructive tools deployed
- **Strategic driver:** International isolation, sanctions, and collapsed nuclear diplomacy pushed Iran to fill intelligence gaps through cyber operations

---

## 3. MITRE ATT&CK Techniques

| Tactic | Technique | ID | Tool / Method |
|--------|-----------|----|---------------|
| Initial Access | Brute Force: Password Spraying | T1110.003 | Manual spray against cloud identities |
| Initial Access | Exploit Public-Facing Application | T1190 | CVE-2022-26134 · CVE-2022-47966 |
| Discovery | Cloud Service Discovery | T1526 | AzureHound · ROADtools |
| Discovery | Account Discovery | T1087 | AD Explorer |
| Persistence | Remote Access Software | T1219 | AnyDesk (abused) |
| Persistence | Cloud Administration Command | T1098 | Azure Arc to attacker subscription |
| Credential Access | Forge Web Credentials: SAML | T1606.002 | Golden SAML |
| Lateral Movement | Remote Services: SMB | T1021.002 | SMB lateral movement |
| Exfiltration | Exfiltration Over C2 Channel | T1041 | Limited cases only |

> Source: Microsoft Threat Intelligence — Sep 2023  
> 📌 Full ATT&CK group profile: [MITRE ATT&CK — APT33 (G0064)](https://attack.mitre.org/groups/G0064/)

---

## 4. Geopolitical Analysis

### Why These Sectors?

**Defense:**  
Compromising defense organizations gives Iran access to weapons system designs, military tactics, command structures, and operational plans — intelligence that directly strengthens its military posture without open conflict.

**Aerospace:**  
The aerospace sector overlaps heavily with missile technology and satellite communications. Stealing data from this sector helps Iran advance its ballistic missile program and understand adversary surveillance capabilities.

**Pharmaceuticals:**  
International sanctions have severely restricted Iran's ability to import advanced medicines and manufacturing processes. Pharmaceutical IP theft allows Iran to replicate drugs domestically — sanctions cannot block what you already know how to make.

---

### Why This Time? (Feb – Jul 2023)

The JCPOA nuclear agreement reached a near-complete collapse between 2022 and 2023:

- **2022:** Nuclear deal negotiations failed due to Iran's hardline demands, domestic unrest (Mahsa Amini protests), and Iran's decision to supply military drones to Russia — all of which deepened its international isolation.
- **Early 2023:** Diplomacy shifted from a comprehensive deal to informal back-channel talks focused on prisoner exchanges and de-escalation management — not sanctions relief.
- **Result:** With no sanctions relief in sight and diplomatic channels effectively closed, Iran turned to **cyber espionage** to obtain what it could no longer acquire through trade or diplomacy.

The campaign timing is not coincidental — it reflects a deliberate strategic decision to use offensive cyber capabilities to fill the intelligence and technology gaps created by Iran's own geopolitical choices.

---

### Espionage vs. Sabotage — How TTPs Confirm Intent

| Indicator | Espionage | Sabotage |
|-----------|-----------|----------|
| Primary goal | Steal data | Destroy / disrupt systems |
| Tools used | AzureHound, ROADtools, AD Explorer | Wipers, ransomware, destructive malware |
| Persistence method | AnyDesk, Azure Arc (stay hidden) | Often not needed — destroy and leave |
| Exfiltration | Yes — selective, targeted | Rare |
| APT33 in this campaign | ✅ Matches | ❌ No destructive tools deployed |

**Conclusion:** The complete absence of wipers or destructive payloads, combined with selective data exfiltration and long-term persistence tooling, confirms this campaign was **pure espionage**. Iran needed information — not leverage.

---

## 5. IOC Reference

| Type | Value | Confidence | Source |
|------|-------|------------|--------|
| IP | `102[.]129.215.40` | Low-Med | Certfa Radar |
| IP | `108[.]62.118.240` | Low-Med | Certfa Radar |
| IP | `192[.]52.166.76` | Low-Med | Certfa Radar |
| IP | `76[.]8.60.64` | Low-Med | Certfa Radar |
| CVE | CVE-2022-26134 | High | Microsoft / NIST NVD |
| CVE | CVE-2022-47966 | High | Microsoft / NIST NVD |
| Tool | AnyDesk | High | Microsoft Threat Intelligence |
| Tool | AzureHound | High | Microsoft Threat Intelligence |
| Tool | ROADtools | High | Microsoft Threat Intelligence |
| Tool | AD Explorer | High | Microsoft Threat Intelligence |

> ⚠️ All IP addresses are defanged. Verify all indicators against current threat feeds before operational use.

---

## 6. References

| # | Source | URL |
|---|--------|-----|
| 1 | Microsoft Threat Intelligence — Peach Sandstorm (Sep 2023) | [link](https://www.microsoft.com/en-us/security/blog/2023/09/14/peach-sandstorm-password-spray-campaigns-enable-intelligence-collection-at-high-value-targets/) |
| 2 | Certfa Radar — Peach Sandstorm Threat Entry (Sep 2023) | [link](https://radar.certfa.com/en/threats/view/3dd41ae0/) |
| 3 | MITRE ATT&CK — APT33 (G0064) | [link](https://attack.mitre.org/groups/G0064/) |
| 4 | NIST NVD — CVE-2022-26134 | [link](https://nvd.nist.gov/vuln/detail/CVE-2022-26134) |
| 5 | NIST NVD — CVE-2022-47966 | [link](https://nvd.nist.gov/vuln/detail/CVE-2022-47966) |

---

*Report produced for educational and portfolio purposes. TLP:WHITE — freely shareable.*
