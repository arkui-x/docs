# Certificate Overview
The **Certificate** module provides APIs for X.509 certificate operations. You can use the APIs provided by this module to easily complete your development.

## Basic Concepts

A digital certificate provides a means of digitally verifying the identity of a user, device, or service. X.509 is an international standard format for public key certificates. The crypto framework provides the following capabilities related to X.509:

- X.509 certificate capabilities: parsing and serializing an X.509 certificate, verifying an X.509 certificate signature, and obtaining certificate information.
- X.509 certificate revocation list (CRL) capabilities: parsing, serializing, and obtaining the X.509 CRL.
- Certificate chain validator capabilities: verifying a certificate chain (excluding the certificate validity period) and obtaining certificate chain algorithms.

## Constraints

- Multi-thread concurrent operations are not supported.

### Certificate Specifications

- Certificate chain verification<br>

  The certificate chain validator does not verify the certificate validity period because the device system time is always untrusted. To check the validity period of a certificate, use **checkValidityWithDate()** of **X509Cert**.

- Certificate formats
  
  Currently, only the certificates in DER and PEM formats are supported.
