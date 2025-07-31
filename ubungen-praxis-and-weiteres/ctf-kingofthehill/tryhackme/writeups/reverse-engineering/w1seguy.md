---
description: https://tryhackme.com/room/w1seguy
---

# W1seGuy

### Skript: Aufgabe

Es wurde ein Python-Skript gegeben, welches die kodierten Texte erzeugt:

{% code title="FLAG-Encoder (serverseitig)" %}
```python
import random
import socketserver 
import socket, os
import string

flag = open('flag.txt','r').read().strip()

def send_message(server, message):
    enc = message.encode()
    server.send(enc)

def setup(server, key):
    flag = 'THM{thisisafakeflag}' 
    xored = ""

    for i in range(0,len(flag)):
        xored += chr(ord(flag[i]) ^ ord(key[i%len(key)]))

    hex_encoded = xored.encode().hex()
    return hex_encoded

def start(server):
    res = ''.join(random.choices(string.ascii_letters + string.digits, k=5))
    key = str(res)
    hex_encoded = setup(server, key)
    send_message(server, "This XOR encoded text has flag 1: " + hex_encoded + "\n")
    
    send_message(server,"What is the encryption key? ")
    key_answer = server.recv(4096).decode().strip()

    try:
        if key_answer == key:
            send_message(server, "Congrats! That is the correct key! Here is flag 2: " + flag + "\n")
            server.close()
        else:
            send_message(server, 'Close but no cigar' + "\n")
            server.close()
    except:
        send_message(server, "Something went wrong. Please try again. :)\n")
        server.close()

class RequestHandler(socketserver.BaseRequestHandler):
    def handle(self):
        start(self.request)

if __name__ == '__main__':
    socketserver.ThreadingTCPServer.allow_reuse_address = True
    server = socketserver.ThreadingTCPServer(('0.0.0.0', 1337), RequestHandler)
    server.serve_forever()
```
{% endcode %}

***

### Skript: Dekodierung

Zum Decoden wurde folgender Skript erstellt:

{% code title="crackWiseGuyFlag.py" %}
```python
def decode_hex(hexhash):
    if len(hexhash) % 2 != 0:
        print(f"[!] Invalid hex length: {len(hexhash)} (must be even)")
        return None

    decoded = ""
    for e in range(0, len(hexhash), 2):
        decoded += chr(int(hexhash[e:e+2], 16))
    return decoded

def crack(keyset, encoded_char, expected_char):
    for key in keyset:
        if chr(ord(key) ^ ord(expected_char)) == encoded_char:
            return key
    return None

def crack_key(decoded):
    keyspace = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'
    expected_prefix = "THM{"
    recovered_key = ""

    for i in range(5):
        target_char = "}" if i == 4 else expected_prefix[i]
        encoded_char = decoded[-1] if i == 4 else decoded[i]

        key_char = crack(keyspace, encoded_char, target_char)
        if key_char:
            recovered_key += key_char
        else:
            recovered_key += "?"
    return recovered_key

def decode_by_key(data, key):
    return ''.join(chr(ord(c) ^ ord(key[i % len(key)])) for i, c in enumerate(data))

# --- Interface ---
print("\n" + "#" * 60)
print("#             XOR D3CRYPT0R - W1seGuy             #")
print("# > shift000")
print("#" * 60 + "\n")

hex_input = input("[+] Enter encoded string: ").strip()
decoded_data = decode_hex(hex_input)

if decoded_data:
    print(f"[*] Hex decoded payload : {decoded_data}")

    derived_key = crack_key(decoded_data)
    print(f"[*] Derived XOR key     : {derived_key}")

    final_output = decode_by_key(decoded_data, derived_key)

    print("\n" + "=" * 60)
    print("[✓] Decryption complete.")
    print(f"[*] Final Output        : {final_output}")
    print("=" * 60)
else:
    print("[!] Aborting due to input error.")

```
{% endcode %}

***

### Skript: Generierung Test-Texten

Zum Generieren von Test-Strings kann folgendes benutzt werden

```python
import random, string

key = str(''.join(random.choices(string.ascii_letters + string.digits, k=5)))

def gen_hex_encoded(flag_string, key):
    enc = ""
    for i in range(0,len(flag_string)):
        enc += chr(ord(flag_string[i]) ^ ord(key[i%len(key)]))
    return enc.encode().hex()
    
print(f"WITH KEY : {key}")
print(f"ENCODED  : {gen_hex_encoded('THM{thisisafakeflag}', key)}")
```

***

### Lösung der Aufgabe

Kommunikation mit dem Server über [netcat.md](../../../../../programme-skripte/netcat.md "mention")

```sh
$ nc $IP 1337
This XOR encoded text has flag 1: 38090317255d2022022129393a2d2118752d07362d2f3c5f34000d3704001e35375c201e39011e28
What is the encryption key? lANlU
Congrats! That is the correct key! Here is flag 2: THM{BrUt3_ForC1nG_XOR_cAn_B3_FuN_nO?}
```

Dekodieren der verschlüsselten Flag

```sh
############################################################
#             XOR D3CRYPT0R - W1seGuy             #
# > shift000
############################################################

[+] Enter encoded string: 38090317255d2022022129393a2d2118752d07362d2f3c5f34000d3704001e35375c201e39011e28
757\ 9( decoded payload : 8	%] "!)9:-!u-6-/<_4
[*] Derived XOR key     : lANlU

============================================================
[✓] Decryption complete.
[*] Final Output        : THM{p1alntExtAtt4ckcAnr3alLyhUrty0urxOr}
============================================================
```
