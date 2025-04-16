# Emlog Pro < 2.5.9 has a file upload vulnerability
Vulnerability Location: /admin/store.php
Affected Range: <= Emlog Pro 2.5.9
Vulnerability Cause: The store.php component contains a critical security flaw where it fails to properly validate the contents of remotely downloaded ZIP plugin files. This insufficient validation allows attackers to execute arbitrary code on the vulnerable system.
Vulnerability Impact: Full Server Takeover
