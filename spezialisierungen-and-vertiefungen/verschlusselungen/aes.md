# AES

**AES (Advanced Encryption Standard)** ist ein symmetrischer Blockchiffre-Algorithmus, standardisiert in **FIPS 197**, der Daten in **128-Bit-Blöcken** verarbeitet. Er unterstützt Schlüssel mit **128, 192 oder 256 Bit** und verwendet je nach Schlüssellänge **10, 12 oder 14 Runden**.

Der zugrunde liegende Algorithmus basiert auf **Substitution-Permutation-Netzwerken (SPN)** und bietet starke Sicherheit gegen klassische Kryptoanalyse (z. B. lineare, differentielle Analyse).

AES ist ein effizienter, hardwarebeschleunigter und kryptographisch robuster Algorithmus. In der Praxis ist weniger der Algorithmus selbst, sondern die **Implementierung (z. B. IV-Handhabung, Moduswahl, Key-Management)** die häufigere Schwachstelle.

### Technische Eigenschaften

* **Blockgröße:** 128 Bit
* **Schlüssellängen:** 128, 192, 256 Bit
* **Runden:** 10 (AES-128), 12 (AES-192), 14 (AES-256)
* **Operationen pro Runde:**
  * SubBytes (nicht-lineare Substitution mit S-Box)
  * ShiftRows (Zeilenverschiebung)
  * MixColumns (lineare Transformation)
  * AddRoundKey (XOR mit Rundenschlüssel)

Die Schlüsselableitung erfolgt über den **AES Key Schedule**.

### Betriebsmodi (Modes of Operation)

Da AES nur feste Blockgrößen verarbeitet, werden Betriebsmodi verwendet, um längere Datenströme zu verschlüsseln:

| Modus   | Beschreibung                            | Sicherheitshinweis                                                 |
| ------- | --------------------------------------- | ------------------------------------------------------------------ |
| **ECB** | Jeder Block einzeln verschlüsselt       | Unsicher, da gleiche Klartextblöcke identisch verschlüsselt werden |
| **CBC** | Verkettung mit IV, Padding erforderlich | Sicher bei korrekter IV-Nutzung, aber Padding-Orakel-anfällig      |
| **CTR** | Zählerbasiert, AES als Stream Cipher    | Parallelisierbar, IV muss einzigartig sein                         |
| **GCM** | CTR + Authentifizierung via Galois-Feld | Authenticated Encryption (AEAD), sehr performant                   |

### Verwendung in der Praxis

AES ist in zahlreichen sicherheitskritischen Technologien implementiert:

* **TLS/SSL**: z. B. AES-GCM in HTTPS
* **IPsec / OpenVPN**: z. B. AES-CBC oder AES-GCM
* **Festplattenverschlüsselung**: BitLocker, LUKS
* **Container/Vaults**: VeraCrypt, Cryptomator
* **Dateiverschlüsselung**: OpenSSL, GPG, S/MIME

