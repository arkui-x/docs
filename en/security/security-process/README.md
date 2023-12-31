# ArkUI-X Community Vulnerability Governance

A vulnerability is a flaw or weakness in a system's design, implementation, or operation and management that could be exploited to violate the system's security policy (RFC 4949).

## ArkUI-X Security Vulnerability Governance and Concept

The ArkUI-X community attaches great importance to the security of the community versions. Despite the industry consensus that security vulnerabilities are inevitable, the ArkUI-X community will adhere to the following principles and actively mitigate potential risks of security vulnerabilities.

- Proactive management: Take proactive measures to minimize security vulnerabilities in products and services.
- Openness and collaboration: cooperate with upstream and downstream communities to provide risk mitigation solutions to customers for security vulnerabilities found in product and services.

## ArkUI-X Community Security Vulnerability Handling Process
The ArkUI-X community has set up a complete security vulnerability handling process in compliance with ISO/IEC 30111 and ISO/IEC 29147 to ensure timely response to community security vulnerabilities and minimize security risks.

### Collecting Security Vulnerabilities

The ArkUI-X community has multiple channels to collect security vulnerabilities from upstream open-source software and native open-source software in the community.


|Source|Channel|Reporting Method|  Description|
| -------- |-------- | -------- | -------- |
|Upstream open-source software|Community contributor|Security vulnerability issue|Community contributors submit issues regarding security vulnerabilities detected in upstream open-source software.|
|Upstream open-source software|Security vulnerability scanning tool|Security vulnerability issue|The release versions are scanned for security vulnerabilities every month, and issues will be submitted if security vulnerabilities are detected.|
|Native open-source software|Security researchers|scy@mail.arkui-x.cn|Upon receiving the [email](./template-security-bug.md), the Security Issue Response Team will create a security issue in the community.|

**Security vulnerability issue**: When finding a security vulnerability, create an issue in the repository where the issue is found, select **Private**, and label the issue **Security**.

### Assessing Security Vulnerabilities

The ArkUI-X Security Issue Response Team organizes maintainers to review the security vulnerabilities reported. The ArkUI-X community assesses security vulnerabilities based on the mainstream [CVSS](https://www.first.org/cvss/calculator/3.1). The table below lists the severity levels and scores of the Common Vulnerability Scoring System (CVSS).

|Severity Rating|Score|
|--------------------------|-----------------|
|Critical|9.0 - 10.0|
|High|7.0 - 8.9|
|Medium|4.0 - 6.9|
|Low|0.1 - 3.9|

### Disclosing Security Vulnerabilities

The ArkUI-X community complies with the principle of responsible disclosure. After security vulnerabilities are fixed, [Security bulletins](../security-disclosure/README.md) will be released.


## [Acknowledgement](./Acknowledgements.md)
