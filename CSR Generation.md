# CSR Generation on Cisco ASA Firewall

## Overview

A Certificate Signing Request (CSR) is generated on the Cisco ASA firewall to request a digitally signed SSL certificate from a trusted Certificate Authority (CA). The CSR contains the device's public key and identity information. The corresponding private key remains securely stored on the firewall and is never shared.

---

## Prerequisites

* Administrative (enable) access to the Cisco ASA firewall.
* Existing RSA key pair or permission to generate a new key pair.
* Maintenance window (recommended).
* Fully Qualified Domain Name (FQDN) of the firewall.
* Certificate Authority (CA) requirements (key length, signature algorithm, etc.).

---

## Step 1 – Verify Existing Configuration

Before creating a new CSR, verify the existing certificate configuration.

```bash
show crypto ca trustpoints

show crypto ca certificates

show crypto key mypubkey rsa

show run ssl
```

Review the following:

* Existing trustpoints
* Installed certificates
* RSA key pairs
* Active SSL trustpoint

---

## Step 2 – Generate an RSA Key Pair

Generate a new RSA key pair if one does not already exist.

```bash
conf t

crypto key generate rsa label <KEY_NAME> modulus 4096

end
```

Example:

```bash
crypto key generate rsa label FW_SSL_KEY modulus 4096
```

**Expected Result**

```
INFO: RSA keypair generated successfully.
```

---

## Step 3 – Create a Trustpoint

Configure a new trustpoint for the certificate request.

```bash
conf t

crypto ca trustpoint <TRUSTPOINT_NAME>

 enrollment terminal

 fqdn firewall.example.com

 subject-name CN=firewall.example.com

 keypair <KEY_NAME>

 exit

end
```

---

## Step 4 – Verify the Trustpoint

```bash
show crypto ca trustpoints
```

A newly created trustpoint should appear before the certificate is imported.

---

## Step 5 – Generate the CSR

Start the certificate enrollment process.

```bash
conf t

crypto ca enroll <TRUSTPOINT_NAME>
```

Typical prompts:

```
Continue with enrollment?        yes

Include device serial number?    no

Display certificate request?     yes
```

---

## Step 6 – Copy the CSR

The firewall displays the CSR in PEM format.

Copy everything between:

```
-----BEGIN CERTIFICATE REQUEST-----

...

-----END CERTIFICATE REQUEST-----
```

Submit this CSR to your Certificate Authority.

---

## Verification

Verify that the trustpoint exists.

```bash
show crypto ca trustpoints
```

The trustpoint remains **Not Authenticated** until a signed certificate is imported.

---

## Notes

* Never share the private key.
* Store the CSR only until the certificate is issued.
* Verify the Common Name (CN) and FQDN before submitting the CSR.
* Use a 4096-bit RSA key when required by your organization's security standards.
