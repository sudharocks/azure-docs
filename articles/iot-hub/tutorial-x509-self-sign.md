---
title: Tutorial - Use OpenSSL to create self signed certificates for Azure IoT Hub | Microsoft Docs
description: Tutorial - Use OpenSSL to create self-signed X.509 certificates for Azure IoT Hub
author: kgremban

ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 12/30/2022
ms.author: kgremban
ms.custom: [mvc, 'Role: Cloud Development', 'Role: Data Analytics']
#Customer intent: As a developer, I want to be able to use X.509 certificates to authenticate devices to an IoT hub. This step of the tutorial needs to show me how to use OpenSSL to self-sign device certificates.
---

# Tutorial: Use OpenSSL to create self-signed certificates

You can authenticate a device to your IoT hub using two self-signed device certificates. This type of authentication is sometimes called *thumbprint authentication* because the certificates contain thumbprints (hash values) that you submit to the IoT hub. The following steps show you how to create two self-signed certificates. This type of certificate is typically used for testing.

## Step 1 - Create a key for the first certificate


```bash
openssl genpkey -out device1.key -algorithm RSA -pkeyopt rsa_keygen_bits:2048
```

## Step 2 - Create a CSR for the first certificate

Make sure that you specify the device ID when prompted.

```bash
openssl req -new -key device1.key -out device1.csr

Country Name (2 letter code) [XX]:.
State or Province Name (full name) []:.
Locality Name (eg, city) [Default City]:.
Organization Name (eg, company) [Default Company Ltd]:.
Organizational Unit Name (eg, section) []:.
Common Name (eg, your name or your server hostname) []:{your-device-id}
Email Address []:.

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:.
An optional company name []:.
```

## Step 3 - Check the CSR

```bash
openssl req -text -in device1.csr -noout
```

## Step 4 - Self-sign certificate 1

```bash
openssl x509 -req -days 365 -in device1.csr -signkey device1.key -out device1.crt
```

## Step 5 - Create a key for the second certificate

```bash
openssl genpkey -out device2.key -algorithm RSA -pkeyopt rsa_keygen_bits:2048
```

## Step 6 - Create a CSR for the second certificate

When prompted, specify the same device ID that you used for certificate 1.

```bash
openssl req -new -key device2.key -out device2.csr

Country Name (2 letter code) [XX]:.
State or Province Name (full name) []:.
Locality Name (eg, city) [Default City]:.
Organization Name (eg, company) [Default Company Ltd]:.
Organizational Unit Name (eg, section) []:.
Common Name (eg, your name or your server hostname) []:{your-device-id}
Email Address []:.

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:.
An optional company name []:.
```

## Step 7 - Self-sign certificate 2

```bash
openssl x509 -req -days 365 -in device2.csr -signkey device2.key -out device2.crt
```

## Step 8 - Retrieve the thumbprint for certificate 1

```bash
openssl x509 -in device1.crt -noout -fingerprint
```

## Step 9 - Retrieve the thumbprint for certificate 2

```bash
openssl x509 -in device2.crt -noout -fingerprint
```

## Step 10 - Create a new IoT device

Navigate to your IoT hub in the Azure portal and create a new IoT device identity with the following characteristics:

* Provide the **Device ID** that matches the subject name of your two certificates.
* Select the **X.509 Self-Signed** authentication type.
* Paste the hex string thumbprints that you copied from your device primary and secondary certificates. Make sure that the hex strings have no colon delimiters.


## Next Steps

Go to [Tutorial: Test certificate authentication](tutorial-x509-test-certificate.md) to determine if your certificate can authenticate your device to your IoT hub. The code on that page requires that you use a PFX certificate. Use the following OpenSSL command to convert your device .crt certificate to .pfx format.

```bash
openssl pkcs12 -export -in device.crt -inkey device.key -out device.pfx
```
