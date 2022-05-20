
title: Adding storage account to AD DS
date: Fri 05/20/2022 

----

## Battling the AD DS legacy

I was tasked with creating a POC for adding Azure Storage Account to Active Directory Directory Services. 
(On-Prem AD). To make this work, I followed Microsofts documentation found [here](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-identity-ad-ds-enable). However, this documentation was
lacking a lot of information for the manual steps. As I was constrained to using Linux, the AD module would
not work, as this is a compiled DLL, only running on windows.
After reading A LOT of documentation regarding BER-encoding, that gave me nothing in return; I found this
[old technet blog from microsoft](https://docs.microsoft.com/en-us/previous-versions/technet-magazine/ff848710(v=msdn.10)?redirectedfrom=MSDN). 
It was posted in 2010, and gave me all the information I needed. I combined the insights from that blog
into a short python script:
```python
#berencode.py
import base64
import sys
def encode_pwd(pw):
    new_pw = b""
    pw = "\"" + pw + "\""
    for char in pw:
        new_pw += char.encode("utf-16le")
    return base64.standard_b64encode(new_pw)
pw = sys.argv[1]
result = encode_pwd(pw)
stripped_result=str(result).strip("b'")
print(stripped_result)
```
I trigger the script from bash, and pipe it's output to openldap. This is an example of replacing unicodePwd
with and admin account. 

```bash
#!/bin/bash
b64pwd=$(python3 berencode.py 'password')
ldapmodify -v -H 'ldaps://host' -U "username" -w "password" -Y DIGEST-MD5 \
<<EOF
dn: "CN=storageaccountName,DC=Contoso,DC=Com"
replace: unicodePwd
unicodePwd::$b64pwd
EOF
echo "$b64pwd"
```

After you have the BER encoded password, just follow along with the original Microsoft documentation. 
I have submitted these additional steps as a pull request for the documenation, so hopefully it will get 
updated, and hopefully it will save those coming after me some time.

Cheers, and happy hacking!

