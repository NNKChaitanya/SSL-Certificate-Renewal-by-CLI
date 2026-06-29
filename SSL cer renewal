# SSL Certificate Import and Activation on Cisco ASA Firewall

## Overview

After the Certificate Authority issues the signed SSL certificate, import it into the Cisco ASA trustpoint that was used to generate the CSR. Once imported, verify the installation and assign the trustpoint to the required interface.

---

## Prerequisites

* Signed SSL certificate in Base64 PEM format.
* Existing trustpoint used during CSR generation.
* Administrative access to the Cisco ASA firewall.

---

## Step 1 – Verify the Trustpoint

```bash
show crypto ca trustpoints
```

Confirm the required trustpoint exists.

---

## Step 2 – Import the Signed Certificate

Start the import process.

```bash
conf t

crypto ca import <TRUSTPOINT_NAME> certificate
```

When prompted, paste the complete certificate.

```
-----BEGIN CERTIFICATE-----

...

-----END CERTIFICATE-----
```

Type:

```
quit
```

Expected output:

```
INFO: Certificate successfully imported.
```

---

## Step 3 – Verify the Imported Certificate

```bash
show crypto ca certificates
```

Verify the following:

* Certificate Status: Available
* Subject Name
* Issuer
* Validity Period
* Associated Trustpoint
* Public Key Size

---

## Step 4 – Verify SSL Configuration

```bash
show run ssl
```

Identify the trustpoint currently assigned to the interface.

Example:

```bash
ssl trust-point OLD_TRUSTPOINT outside
```

---

## Step 5 – Activate the New Certificate

Replace the old trustpoint with the new trustpoint.

```bash
conf t

no ssl trust-point OLD_TRUSTPOINT outside

ssl trust-point NEW_TRUSTPOINT outside

end
```

---

## Step 6 – Save the Configuration

```bash
write memory
```

---

## Step 7 – Validate the Deployment

Verify:

* HTTPS/ASDM is accessible.
* Browser displays the new certificate.
* Certificate validity dates are correct.
* Certificate chain is trusted.
* No SSL warnings are displayed.

---

## Rollback

If required, restore the previous trustpoint.

```bash
conf t

no ssl trust-point NEW_TRUSTPOINT outside

ssl trust-point OLD_TRUSTPOINT outside

end

write memory
```

---

## Best Practices

* Perform certificate replacement during a maintenance window.
* Verify the certificate before activation.
* Keep the previous trustpoint until validation is complete.
* Backup the running configuration before making changes.
* Never publish production certificates, CSRs, private keys, or internal hostnames in public repositories.
