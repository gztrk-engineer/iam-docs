# Technical security overview

You take the information security seriously and don’t want your service to be compromised.
Meanwhile, your organization tries to strike a balance between the business objectives and the
users’ preferences, while meeting the targets in confidentiality, integrity, and authenticity.

If you have a mobile app, your users' actions need to be authorized and accounted for. Evidently,
a single factor authentication is not enough, but developing multi-factor authentication
(MFA) in house and ensuring a smooth user experience at the same time is a tall order.

Our mobile SDK enables your business to authenticate the customers and workforce
personnel using a mobile device and all supported verification methods. Moreover, you can
complement the authentication with mobile threat detection (for example, detect jailbroken or
rooted devices) and have the option to manage the user devices via the device management
portal.

This document aims to explain how Callsign’s mobile SDK can help you reduce the attack
surface and manage security risks. 

## Functionality  

The mobile SDK enables you to orchestrate web and mobile user authentication journeys,
manage the device authenticators and collect related user and device data for risk analysis and
machine learning.

The mobile SDK provides the following authentication methods for mobile and cross-channel
scenarios:

SDK hosted methods | Other authenticators 
---|---  
IAM Biometrics <br> IAM Device Ownership <br> IAM Pin <br> IAM Behavioral Pin <br> IAM Path <br> IAM Behavioral Path <br> IAM Soft Tokens | SMS One Time Password (OTP) <br> Email OTP <br> Temporary Access Codes (TACs) <br> Hardware Token OTP <br> Externally Managed Authenticators

You can use the mobile SDK to authenticate users on other channels where the platform has
initiated a logon via [Radius](), [SAML](), or [OIDC]().  