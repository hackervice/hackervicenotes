# Active Directory

### LLMNR Poisining

{% code title="PowerSHell" %}
```powershell
$(Get-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows NT\DNSClient" -name EnableMulticast).EnableMulticast
```
{% endcode %}

{% code title="via cmd" %}
```powershell
wmic nicconfig get caption, index, c
```
{% endcode %}

