<a name="virtual-networking-limits-classic"></a>Följande begränsningar gäller endast för nätverksresurser som hanteras via den klassiska distributionsmodellen per prenumeration.

| Resurs | Standardgräns | Övre gräns |
| --- | --- | --- |
| Virtuella nätverk |50 |100 |
| Lokala nätverksplatser |20 |kontakta supporten |
| DNS-servrar per virtuellt nätverk |20 |100 |
| Privata IP-adresser per virtuellt nätverk |4096 |4096 |
| Samtidiga TCP eller UDP flödar per nätverkskort på en virtuell dator eller rollinstans |500 000 |500 000 |
| Nätverkssäkerhetsgrupper (NSG) |100 |200 |
| NSG-regler per NSG |200 |400 |
| Användardefinierade vägtabeller |100 |200 |
| Användardefinierade vägar vägtabell |100 |400 |
| Offentliga IP-adresser (dynamiska) |5 |kontakta supporten |
| Reserverade offentliga IP-adresser |20 |kontakta supporten |
| Offentlig VIP per distribution |5 |kontakta supporten |
| Privat VIP (ILB) per distribution |1 |1 |
| Åtkomstkontrollistor (ACL:er) för slutpunkt |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Nätverksgränser – Azure Resource Manager
Följande begränsningar gäller endast för nätverksresurser som hanteras via Azure Resource Manager per region per prenumeration.

| Resurs | Standardgräns | Övre gräns |
| --- | --- | --- |
| Virtuella nätverk |50 |1000 |
| Undernät per virtuellt nätverk |1000 |10000 |
| Peerkopplingar mellan virtuella nätverk per virtuellt nätverk |10 |50 |
| DNS-servrar per virtuellt nätverk |9 |25 |
| Privata IP-adresser per virtuellt nätverk |4096 |8192 |
| Privata IP-adresser per nätverksgränssnitt |256 |1024 |
| Samtidiga TCP eller UDP flödar per nätverkskort på en virtuell dator eller rollinstans |500 000 |500 000 |
| Nätverksgränssnitt (NIC) |350 |20000 |
| Nätverkssäkerhetsgrupper (NSG) |100 |5000 |
| NSG-regler per NSG |200 |500 |
| IP-adresser och intervall som angetts för källan eller målet i en säkerhetsregel |2000 |4000 |
| Programsäkerhetsgrupper |200 |500 |
| Programmet säkerhetsgrupper per IP-konfiguration per NIC |10 |20 |
| IP-konfigurationer per program säkerhetsgrupp |1000 |4000 |
| Säkerhetsgrupper för program som kan anges i alla säkerhetsregler för en nätverkssäkerhetsgrupp |50 |100 |
| Användardefinierade vägtabeller |100 |200 |
| Användardefinierade vägar vägtabell |100 |400 |
| Offentliga IP-adresser – dynamisk |60 (grundläggande) |kontakta supporten |
| Offentliga IP-adresser – statiska |20 (grundläggande) |kontakta supporten |
| Offentliga IP-adresser – statiska |20 (standard) |kontakta supporten |
| Punkt-till-plats-rotcertifikat per VPN-gateway |20 |20 |

#### <a name="load-balancer"></a>Belastningsutjämnaren begränsar

| Resurs | Standardgräns | Övre gräns |
| --- | --- | --- |
| Belastningsutjämnare | 100 | 1000 |
| Regler per resurs, Basic | 150 | 250 |
| Regler per resurs, Standard | 1250 | 1500 |
| Regler per IP-konfiguration | 299 |299 |
| Frontend-IP-konfigurationer, Basic | 10 | kontakta supporten |
| Frontend-IP-konfigurationer, Standard | 10 | 600 |
| Serverdelspool Basic | 100, enkel Tillgänglighetsuppsättning | - |
| Serverdelspool, Standard | 1000, enkel VNet | kontakta supporten |
| Hög tillgänglighet portar, Standard | 1 per interna klientdel | - |

[Kontakta supporten](../articles/azure-supportability/resource-manager-core-quotas-request.md ) om du måste öka gränserna från standardvärdet.

