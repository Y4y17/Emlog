# Emlog Pro <= 2.5.12 has a file upload vulnerability
##### Vulnerability Location: /admin/template.php
##### Affected Range: <= Emlog Pro 2.5.12
##### Vulnerability Cause: Template.php contains a serious security vulnerability that allows an attacker to upload malicious template files, resulting in remote code execution vulnerabilities.
##### Vulnerability Impact: Full Server Takeover
##### Link:https://github.com/emlog/emlog/releases/tag/pro-2.5.12
# Vulnerability recurrence
Download the source code, using phpstudy structures, environment, the interface is as follows:

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/0c2b1f03-a093-4d0b-a7b2-d0099d1b78c2" />

Locally create a folder named 1 with the contents of the malicious file header.php, and then compress the folder into a zip package

<img width="609" alt="image" src="https://github.com/user-attachments/assets/002eac88-d3e4-44a7-807b-ffc66ddb2838" />

Upload the compressed packet

<img width="1497" alt="image" src="https://github.com/user-attachments/assets/e0cebf09-7218-43be-be31-dca8c63cf9a0" />

After successful upload, as follows:

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/5223b6ab-0eb1-41d1-b17f-61260262d0e0" />

Try to access the address: http://10.211.55.4:82/content/templates/1/header.php

<img width="1512" alt="image" src="https://github.com/user-attachments/assets/c824feef-a3ca-48ec-88fd-32cdca122711" />
# Code Analysis
`/admin/template.php`
<img width="948" alt="image" src="https://github.com/user-attachments/assets/b62c11b0-3880-45d1-a00d-e4064e3ca988" />

The uploaded file will be processed by calling the emUnZip function, which will be followed into the function:

<img width="911" alt="image" src="https://github.com/user-attachments/assets/068cd440-0a77-4f0d-b1cc-f76d44237d85" />

The file name is fixed, and the file content is not completely verified, resulting in a remote command execution vulnerability
