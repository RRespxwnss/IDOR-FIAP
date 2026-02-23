# Description
A public endpoint has been identified that allows certificate lookup using a predictable identifier (certificate code), without any authentication or authorization mechanism.

By enumerating this identifier, an attacker can access third-party personal data, including full name, CPF (Brazilian individual taxpayer registration number), RG (Brazilian identity card number), date of birth, and place of birth.

It is also observed that, even when certificate holders choose to publicly share their diplomas with partially obfuscated personal data (for example, on professional networks or other platforms), the certificate identifier often remains visible. From this identifier, it is possible to directly query the endpoint and obtain the complete data, entirely bypassing the obfuscation mechanism implemented in the interface.

# Affected endpoint
https://www.fiap.com.br/consultadocumento

# Payload example
8-character identifier

# Proof of Concept (PoC)
For proof-of-concept (PoC) purposes, searches were conducted using Google dorks techniques to locate publicly available images of certificates. Additionally, examples were identified on LinkedIn profiles where certificate holders shared their certificates.

From these public sources, it was possible to extract the identifiers of the visible certificates and, based on them, assemble a list of valid codes. Using these identifiers on the vulnerable endpoint, it was possible to access the complete data associated with each certificate, demonstrating the feasibility of exploitation at scale.

It should be noted that the technique employed used exclusively public information and passive reconnaissance, demonstrating that an attacker does not need privileged access to exploit the vulnerability.

See the example below.



<img width="1194" height="937" alt="image" src="https://github.com/user-attachments/assets/635447ac-280f-48ac-986f-74a202dafa49" />
<img width="1193" height="936" alt="image" src="https://github.com/user-attachments/assets/cfddd886-7649-47aa-bf39-fbadfc655cc8" />
<img width="1310" height="963" alt="image" src="https://github.com/user-attachments/assets/786ac575-38db-467f-a87a-9bc4e3525790" />
<img width="958" height="1031" alt="image" src="https://github.com/user-attachments/assets/f29ef9b6-f05b-4531-8aa1-746a457ca5af" />
<img width="956" height="1032" alt="image" src="https://github.com/user-attachments/assets/3d8ba923-5e4f-4517-975a-3dec054ce166" />
<img width="1081" height="909" alt="image" src="https://github.com/user-attachments/assets/41f33a2c-cf0e-4b5f-866f-10da50da3d72" />
<img width="1363" height="876" alt="image" src="https://github.com/user-attachments/assets/7665ce72-b1a6-4e5b-9b09-4a7b08720dca" />
<img width="990" height="878" alt="image" src="https://github.com/user-attachments/assets/b9fd91bc-ffcc-41f5-b683-11d627853f2d" />
<img width="995" height="889" alt="image" src="https://github.com/user-attachments/assets/854373c4-6b5f-46da-8d20-b2a09afef3f6" />


# Impact
The identified vulnerability allows unauthorized access to highly sensitive personal data, including CPF (Brazilian taxpayer ID), RG (Brazilian national ID), full name, date of birth, and place of birth, without any authentication required. The possibility of enumerating the certificate identifier, coupled with the collection of these identifiers from public sources, enables the extraction of information on a large scale, significantly expanding the exposure surface.

This scenario allows for the misuse of data for fraud, identity theft, and highly targeted social engineering attacks, since the exposed information is widely used as a validation mechanism in the Brazilian context. Furthermore, the flaw could result in significant regulatory impacts, especially in the context of the General Data Protection Law (LGPD), considering the exposure of personal data without an adequate legal basis or access control.

Finally, it is noteworthy that the exploitation does not require any level of privilege or authentication and can be carried out by external agents in an automated manner, which significantly increases the criticality of the vulnerability.

# Mitigation
To mitigate the risk of enumeration, it is recommended that the certificate identifier be replaced with an unpredictable value, such as a securely generated token (e.g., hash-based like MD5 or, preferably, more robust algorithms with a random component), ensuring that it is not possible to infer or enumerate other valid certificates.

Additionally, it is recommended that public documents present sensitive data properly obfuscated, and that this same logic be applied to the backend, preventing the API from returning complete information. In this way, even if the link is shared publicly, access will be limited to a secure version of the certificate, preserving the holder's privacy.

Finally, it is important to implement complementary protection mechanisms, such as rate limiting, monitoring of suspicious accesses, and, when applicable, the use of verification links with temporary validity, further reducing the risk of automated exploitation.
