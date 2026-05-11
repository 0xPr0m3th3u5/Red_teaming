# NTLM Authentication

Aside from kerberos and LDAP, AD uses several other authentication methods. 
- LM, NT (Hash alogrithms used to store password credentials)
- NTLMv1 (Authentication protocol built on top of LM)
- NTLMv2 (Authentication protocol built on top of NT)

# Lan Manager (LM)

- Oldest password storage mechanism.
- Stored in the `SAM database` on a windows host and the `NTDS.DIT` database on a Domain Controller.
- It has been turned off since windows vista 2008.
- password using LM are limited to a maximum of 14 characters; not case sensitive.
---
- 14 character password is first split into two seven character chunks.
- If it's less than 14, it will be padded with NULL characters to reach the correct value. 
- Two DES keys are generated from chunks. These chunks are encrypted using the string `KGS!@#$%` creating two 8-byte ciphertext values.

# NThash (NTLM)

NT LAN Manager is used in modern windows systems. 

- a challenge response authentication protocol and uses three messages to authenticate.
1. Client first sends a `NEGOTIATE_MESSAGE` to the server.
2. The server replies back with a `CHALLENGE_MESSAGE` to verify the client's identity.
3. Lastly, the client responds with an `AUTHENTICATE_MESSAGE`

- The hashes are stored locally in the `SAM database` or the `NTDS.DIT` database file on a domain controller.

- The protocol has two hashed password values to choose from to perform authentication: LM hash and the NT hash (MD4 hash of the little-endian UTF-16); visualized as `MD4(UTF-16-LE(passowrd))`

- NTLM is often vulnerabile to **pass-the-hash** attack which means the attacker can authenticate to the target systems without knowing the plaintext.

NOTE: Neither LANMAN nor NTLM uses a salt.

for example: 
```
Rachel:500:aad3c435b514a4eeaad3b935b51304fe:e46b9e548fa0d122de7f59fb6d48eaa2:::
```

- Rachel: username
- 500: RID; for administrator mostly
- first part is the LM hash
- second part is the NT hash.

NOTE: The NT hash can be cracked offline to reveal the password value or doing the pass-the-hash attack using `crackmapexec`

```
crackmapexec smb 10.129.41.19 -u rachel -H e46b9e548fa0d122de7f59fb6d48eaa2
```

## NTLMv1

- NTLMv1 uses both the NT hash and LM hash which makes it easier to crack offline after tools such as **responder** or via an **NTLM relay attack**

1. The server sends the client an `8-byte` random number
2. The client returns a `24-byte` response.

NOTE: These hashes can NOT be used for pass-the-hash attacks.

```
C = 8-byte server challenge, random
K1 | K2 | K3 = LM/NT-hash | 5-bytes-0
response = DES(K1,C) | DES(K2,C) | DES(K3,C)
```
u4-netntlm::kNS:338d08f8e26de93300000000000000000000000000000000:9526fb8c23a90751cdd619b6cea564742e1e4bf33006ba41:cb8086049ec4736c
```

## NTLMv2

NTLMv2 sends two responses to the 8-byte challenge received by the server.
these contains: `16-byte HMAC-MD5` hash of the challenge, a randomly generated challenge from the client and an HMAC-MD5 hash of the user's crentials.

```
SC = 8-byte server challenge, random
CC = 8-byte client challenge, random
CC* = (X, time, CC2, domain name)
v2-Hash = HMAC-MD5(NT-Hash, user name, domain name)
LMv2 = HMAC-MD5(v2-Hash, SC, CC)
NTv2 = HMAC-MD5(v2-Hash, SC, CC*)
response = LMv2 | CC | NTv2 | CC*
```

**NTLMv2 example:**

```
admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c78303000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030
```

# Domain Cached Credentials (MSCache2)

If the host cannot reach the domain controller using the previous authentication methods:
- Hosts save the last `ten` hashes for any domain users successfully log into the machine in `HKEY_LOCAL_MACHINE\SECURITY\CACHE` registry key (pass-the-hash cannot be used)
- These hashes can be obtained after gaining local admin access to a host and have the format: 
`$DCC2$10240#bjones#e4e938d12fe5974dc42a90120bd9c90f`
