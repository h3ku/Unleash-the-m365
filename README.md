# Unleash the m365
This repository contains multiple files regarding the investigation about the Xiaomi Mijia M365 Scooter.

**Table of Contents**

* [Unleash the m365](#unleash-the-m365)
    * [Research](#research)
		* [Mi Home](#mi-home)
			* [Android](#android)
				* [APP <--> Server Communication](#app-server-communication)
			* [IOS](#ios)
	* [Downgrade](#downgrade)
	* [Firmware Mods](#firmware-mods)
	* [Hardware Mods](#hardware-mods)
	* [Files Index](#files-index)
	* [License](#license)
	* [Acknowledgments](#acknowledgments)

## Research
This section contains information regarding the research about things like the official apps or the Bluetooth communication protocol.

### Mi Home
#### Android
##### APP Server Communication
All the requests performed by the app to the servers regarding with the scooter (Check updates etc) are ciphered using RC4, in the case of the latest android version all the parameters used to create the key are sent on every request.

```
data=zqnK2TlkGDq6iZToITYyt9CAEtoWht4LEh88XRnU&rc4_hash__=u8lit+iwOqc0P1k+VsRSlZ72POBvp701tEWFSg==&signature=A/l2lbo3OOdt/VoyEyLECfI/6BY=&_nonce=pFBJTd2vcbUBgaHP&ssecurity=Tw+X976Vymge9yBtgZPeMQ==
```

The paremeters used to create the key are "_ssecurity_" and "_\_nonce_" in the following way.
___B64Encode(Sha256(concat(B64Decode(ssecurity), B64Decode(\_nonce))))___

Then this key is B64Decoded and used in a common RC4 algorithm but with 1024 'fake rounds', the following snippet shows a python implementation of this RC4 algorithm.

```python
#b64decoded data and key
def rc4mi(data, key):
    S, j, out = list(range(256)), 0, []

    for i in range(256):
        j = (j + ord(key[i % len(key)]) + S[i]) % 256
        S[i], S[j] = S[j], S[i]

    # 1024 fake rounds
    i = j = 0
    for x in range(1024):
        i = (i + 1) % 256
        j = (j + S[i]) % 256
        S[i], S[j] = S[j], S[i]

    for ch in data:
        i = (i + 1) % 256
        j = (j + S[i]) % 256
        S[i], S[j] = S[j], S[i]
        out.append(chr(ord(ch) ^ S[(S[i] + S[j]) % 256]))
```

#### IOS

## Downgrade

## Firmware Mods

## Hardware Mods

## Files Index

## License
This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments
* Hector Cuesta [H3ku](https://twitter.com/HectorCuesta)
* Jesus Anton [Patatas Fritas](https://twitter.com/HackingPatatas)
* Borja Martinez [Borjmz](https://twitter.com/Qm9yamFN)

