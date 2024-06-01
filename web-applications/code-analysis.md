# ⌨️ Code Analysis

### Prettier

{% embed url="https://prettier.io/playground/" %}

### Beautifier

{% embed url="https://beautifier.io/" %}

### UnPacker

{% embed url="https://matthewfl.com/unPacker.html" %}

### Encoding and Decoding

#### Base64

{% code title="Encoding" %}
```bash
echo http://www.example.com | base64

aHR0cDovL3d3dy5leGFtcGxlLmNvbQo=
```
{% endcode %}

{% code title="Decoding" %}
```bash
echo aHR0cDovL3d3dy5leGFtcGxlLmNvbQo= | base64 -d

http://www.example.com
```
{% endcode %}

#### Hex

{% code title="Encoding" %}
```bash
echo http://www.example.com | xxd -p
           
687474703a2f2f7777772e6578616d706c652e636f6d0a
```
{% endcode %}

{% code title="Decoding" %}
```bash
echo 687474703a2f2f7777772e6578616d706c652e636f6d0a | xxd -p -r

http://www.example.com
```
{% endcode %}

#### Caesar/Rot13

{% code title="Encoding" %}
```bash
echo http://www.example.com | tr 'A-Za-z' 'N-ZA-Mn-za-m'
    
uggc://jjj.rknzcyr.pbz
```
{% endcode %}

{% code title="Decoding" %}
```bash
echo uggc://jjj.rknzcyr.pbz | tr 'A-Za-z' 'N-ZA-Mn-za-m'

http://www.example.com
```
{% endcode %}

#### Cypher Identifier

{% embed url="https://www.boxentriq.com/code-breaking/cipher-identifier" %}
