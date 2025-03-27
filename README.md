# Email Server Security: SPF, DKIM, and DMARC

This document explains the importance of email security using three key protocols: **SPF (Sender Policy Framework)**, **DKIM (DomainKeys Identified Mail)**, and **DMARC (Domain-based Message Authentication, Reporting, and Conformance)**. These protocols help prevent email spoofing, phishing attacks, and ensure the authenticity of email messages.

## Table of Contents

1. [Introduction](#introduction)
2. [SPF: Sender Policy Framework](#spf-sender-policy-framework)
3. [DKIM: DomainKeys Identified Mail](#dkim-domainkeys-identified-mail)
4. [DMARC: Domain-based Message Authentication, Reporting, and Conformance](#dmarc-domain-based-message-authentication-reporting-and-conformance)
5. [How SPF, DKIM, and DMARC Work Together](#how-spf-dkim-and-dmarc-work-together)
6. [Setting Up SPF, DKIM, and DMARC](#setting-up-spf-dkim-and-dmarc)
7. [Official Docs](#official-docs)
8. [License](#licence)
9. [Disclaimer](#disclaimer)


## Introduction

The widespread use of email has also led to an increase in malicious activities, including phishing, spam, and spoofing. To combat these threats, several email authentication protocols have been developed. The most widely adopted are:

- **SPF**: Prevents email spoofing by verifying the sender’s IP address.
- **DKIM**: Ensures the integrity and authenticity of the email content.
- **DMARC**: Enhances SPF and DKIM by providing policy enforcement and reporting.

---

## SPF: Sender Policy Framework

**Sender Policy Framework (SPF)** is an email validation system designed to detect email spoofing by verifying the sender's IP address. SPF enables the domain owner to specify which mail servers are allowed to send email on behalf of the domain. This helps prevent unauthorized mail servers from sending emails that appear to be from your domain.

### How SPF Works:
1. The recipient mail server checks the SPF record in the DNS (Domain Name System) to verify if the sender's IP address matches the allowed mail servers for the domain.
2. If the sender’s IP is authorized, the email is delivered.
3. If not, the email can be marked as suspicious or rejected.

### Setting Up SPF:
To set up SPF, you need to create a TXT record in your domain's DNS. Here’s an example of an SPF record:

```
v=spf1 ip4:10.0.0.0/24 include:spf.protection.outlook.com ~all
```

- `v=spf1` specifies the SPF version.
- `ip4:10.0.0.0/24` allows the specified IP range to send emails.
- `include:spf.protection.outlook.com` allows emails from another domain.
- `~all` marks all other sources as "soft fail."

---

## DKIM: DomainKeys Identified Mail

**DomainKeys Identified Mail (DKIM)** is an email authentication method that uses cryptographic signatures to verify the authenticity of email messages. DKIM allows the sender to sign their emails with a private key, while the recipient can verify the signature using a public key published in the domain’s DNS.

### How DKIM Works:
1. The sender’s mail server generates a cryptographic signature using the private key and adds it to the email header.
2. The recipient mail server retrieves the public key from the sender’s DNS record to validate the signature.
3. If the signature matches, the email is authenticated as originating from the correct domain.

### Setting Up DKIM:
To set up DKIM, you need to generate a pair of private and public keys. The public key is published in your DNS, and the private key is used to sign outgoing emails.

Here’s an example of a DKIM TXT record in your DNS:

```
default._domainkey.example.com  IN  TXT  "v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA7MkEzZ...";
```

- `v=DKIM1` specifies the DKIM version.
- `k=rsa` specifies the key type.
- `p=MIIBIjAN...` is the public key used for signature verification.

---

## DMARC: Domain-based Message Authentication, Reporting, and Conformance

**Domain-based Message Authentication, Reporting, and Conformance (DMARC)** builds on SPF and DKIM by adding a policy layer that allows domain owners to define how email receivers should handle authentication failures. DMARC also provides reporting mechanisms to help monitor and improve email authentication.

### How DMARC Works:
1. The domain owner publishes a DMARC policy in the DNS, which tells recipient servers what to do when an email fails SPF or DKIM checks (e.g., reject, quarantine, or allow).
2. DMARC allows the sender to receive reports from recipient mail servers about the authentication results.

### Setting Up DMARC:
To set up DMARC, you need to create a TXT record in your domain's DNS. Here’s an example of a DMARC record:

```
_dmarc.example.com  IN  TXT  "v=DMARC1; p=reject; rua=mailto:dmarc-reports@example.com"
```

- `v=DMARC1` specifies the DMARC version.
- `p=reject` tells the recipient server to reject emails that fail SPF or DKIM.
- `rua=mailto:dmarc-reports@example.com` specifies where to send aggregate reports.

---

## How SPF, DKIM, and DMARC Work Together

When properly configured, SPF, DKIM, and DMARC work together to create a multi-layered defense against email spoofing:

- **SPF** validates the sender's IP address, ensuring the message is sent from an authorized server.
- **DKIM** adds an encrypted signature to verify the integrity and authenticity of the email content.
- **DMARC** ties SPF and DKIM together, providing policy enforcement and reporting for better security visibility.

By using all three protocols, you can significantly reduce the chances of malicious actors successfully spoofing your domain.

---

## Setting Up SPF, DKIM, and DMARC

Here are the steps for setting up SPF, DKIM, and DMARC for your domain:

1. **Configure SPF**: Add an SPF TXT record to your domain’s DNS. List the authorized mail servers.
2. **Configure DKIM**: Generate a DKIM key pair, publish the public key in your DNS, and sign outgoing emails with the private key.
3. **Configure DMARC**: Create a DMARC policy TXT record in your DNS to define how authentication failures should be handled and where reports should be sent.

Be sure to test your email configuration after setting up these protocols using tools like [MXToolbox](https://mxtoolbox.com/) or [DMARC Analyzer](https://www.dmarcanalyzer.com/).

---

## Official Docs

For additional resources, visit:
- [SPF Official Documentation](https://www.openspf.org/)
- [DKIM Official Documentation](https://dkim.org/)
- [DMARC Official Documentation](https://dmarc.org/)

---

## License

This project is licensed under the MIT License. See the LICENSE file for more details.

---

## Disclaimer

This project is provided as-is, and the authors are not responsible for any damages or losses resulting from its use. Always test sec
urity measures in a staging environment before applying them to a prod site.

