# Windows Server és Kliens Integrációs Labor v2.0

## Projekt Összefoglaló
A projekt célja egy olyan elkülönített és magas rendelkezésre állású vállalati laboratóriumi környezet felépítése VirtualBoxban, amely a korábbi egykiszolgálós alapokra építve már redundáns tartományi szolgáltatásokat is tartalmaz[cite: 1]. A munka középpontjában a két tartományvezérlő közötti Active Directory és DNS replikáció, a DHCP Failover hibatűrés, a DFS és DFSR alapú replikált fájlmegosztás, a központi csoportházirendek, valamint a belső PKI és a HTTPS webszerver kialakítása áll[cite: 1].

## Alkalmazott Technológiák és Környezet
- **Virtualizáció:** Oracle VirtualBox[cite: 1]
- **Elsődleges szerver, SRV-DC01:** Windows Server 2022, amely Domain Controller, DNS, DHCP Failover, fájlszerver és DFSR, AD CS Enterprise Root CA, valamint IIS HTTPS szerepeket lát el, `10.0.2.10` IP-címmel[cite: 1]
- **Másodlagos szerver, SRV-DC02:** Windows Server 2022, amely Domain Controller, DNS, DHCP Failover és DFSR szerepeket lát el, `10.0.2.11` IP-címmel[cite: 1]
- **Kliens gép, CLI-WIN11:** Windows 11 Enterprise tartományi kliens, amely DHCP-n keresztül kap IP-címet, például `10.0.2.50`[cite: 1]
- **Hálózati beállítás:** Internal Network `intnet` és NAT Network[cite: 1]

## Főbb Konfigurációs Lépések
1. **Szerver alapbeállítások és redundancia:** Statikus IP-címek és gépnevek beállítása mindkét szerveren, `SRV-DC01` és `SRV-DC02` néven, majd a DHCP Failover beállítása a hibatűrő IP-kiosztáshoz[cite: 1].
2. **Active Directory és DNS telepítés:** Az AD DS és az integrált DNS szerepkörök telepítése, az új `DomCont.local` erdő létrehozása, majd a másodlagos tartományvezérlő beléptetése a címtár és a DNS-zónák replikálásához[cite: 1].
3. **Szervezeti struktúra és jogosultságkezelés:** Az `Enterprise` fő OU alatt a `Servers`, `Workstations` és `Users` egységek kialakítása, ezen belül `Admin`, `IT`, `HR` és `Sales` ágakkal, valamint a `GRP-IT` és `GRP-HR` biztonsági csoportok és a tesztfiókok létrehozása[cite: 1].
4. **Tárolás, DFS replikáció és GPO-alapú meghajtók:**
   - A HR részleg saját megosztása `\\SRV-DC01\HR_Share` néven, NTFS és megosztási jogosultságokkal, valamint a `Map-Hr-Drive` házirenddel, amely *Item-level Targeting* segítségével csatolja fel a `H:` meghajtót `HR_Storage` néven[cite: 1].
   - Tartományi szintű DFS névtér a `\\DomCont.local\Corp\DocumentsShared` útvonalon, kétirányú DFSR replikációval a két szerver között, és automatikus `Z:` meghajtó csatolással[cite: 1].
5. **Központi házirendek és szoftverterjesztés:** Bejelentkezés előtti biztonsági figyelmeztetés, az *Enterprise Labor* üzenet, és a Notepad++ automatikus telepítése csoportházirenden keresztül[cite: 1].
6. **Belső PKI és IIS HTTPS:** A `DomCont-SRV-DC01-CA` Enterprise Root CA telepítése, egy egyedi webkiszolgáló tanúsítványsablon létrehozása, majd SSL-tanúsítvány igénylése és hozzárendelése az IIS webszerverhez a `https://srv-dc01.domcont.local` címen[cite: 1].
7. **Kliens integráció és tesztelés:** A Windows 11 kliens tartományhoz csatlakoztatása, a DHCP Failover kipróbálása, valamint a házirendek, a hálózati meghajtók és a HTTPS elérés ellenőrzése[cite: 1].

## Megjegyzés a Laborkörnyezetről
A labor erőforrásaihoz igazodva több szerepkör, például a fájlszerver, a CA és az IIS is a tartományvezérlőkön fut, a megosztások a `C:` meghajtón vannak, a tartomány pedig `.local` végződést használ[cite: 1]. Éles vállalati környezetben ezeket különálló szerverekkel, dedikált adattároló meghajtókkal és bejegyzett aldomainnel érdemes kiváltani[cite: 1].

## Részletes Dokumentáció
A lépésről lépésre haladó leírás és a tesztelési eredmények a képernyőképekkel együtt a mellékelt PDF vagy DOCX dokumentumban találhatók[cite: 1].

---

# Windows Server and Client Integration Lab v2.0

## Project Overview
This project builds an isolated, high-availability enterprise lab environment in VirtualBox, expanding on the earlier single-server setup with redundant domain services[cite: 1]. It focuses on dual-DC Active Directory and DNS replication, DHCP Failover fault tolerance, DFS and DFSR replicated file sharing, centralized Group Policies, and running an internal PKI alongside an HTTPS web server[cite: 1].

## Technologies and Environment Used
- **Virtualization:** Oracle VirtualBox[cite: 1]
- **Primary Server, SRV-DC01:** Windows Server 2022 running Domain Controller, DNS, DHCP Failover, File Server and DFSR, AD CS Enterprise Root CA, and IIS HTTPS at `10.0.2.10`[cite: 1]
- **Secondary Server, SRV-DC02:** Windows Server 2022 running Domain Controller, DNS, DHCP Failover, and DFSR at `10.0.2.11`[cite: 1]
- **Client Machine, CLI-WIN11:** Windows 11 Enterprise domain client receiving an IP via DHCP, such as `10.0.2.50`[cite: 1]
- **Network Settings:** Internal Network `intnet` and NAT Network[cite: 1]

## Key Configuration Steps
1. **Server Setup and Redundancy:** Configuring static IP addresses and hostnames on both servers, `SRV-DC01` and `SRV-DC02`, then setting up DHCP Failover for fault-tolerant IP leasing[cite: 1].
2. **Active Directory and DNS Setup:** Installing the AD DS and integrated DNS roles, creating the new `DomCont.local` forest, and joining the secondary domain controller to replicate directory data and DNS zones[cite: 1].
3. **Organizational Structure and Access Control:** Creating the `Enterprise` root OU with `Servers`, `Workstations`, and `Users` containing `Admin`, `IT`, `HR`, and `Sales`, along with the `GRP-IT` and `GRP-HR` security groups and test accounts[cite: 1].
4. **Storage, DFS Replication, and GPO Drives:**
   - A dedicated HR share at `\\SRV-DC01\HR_Share` with NTFS and share permissions, mapped automatically as drive `H:` named `HR_Storage` through the `Map-Hr-Drive` Group Policy using *Item-level Targeting*[cite: 1].
   - A domain-based DFS Namespace at `\\DomCont.local\Corp\DocumentsShared` with two-way DFSR replication between both servers, mapped automatically as drive `Z:`[cite: 1].
5. **Group Policies and Software Deployment:** Configuring a pre-login security notice with the *Enterprise Labor* message and automatic Notepad++ installation via Group Policy[cite: 1].
6. **Internal PKI and IIS HTTPS:** Installing the `DomCont-SRV-DC01-CA` Enterprise Root CA, creating a custom web server certificate template, and requesting and binding an SSL certificate for IIS at `https://srv-dc01.domcont.local`[cite: 1].
7. **Client Integration and Testing:** Joining the Windows 11 client to the domain, testing DHCP Failover, and verifying applied policies, mapped drives, and trusted HTTPS access[cite: 1].

## Lab Environment Note
To keep the lab resource-friendly, multiple roles including file sharing, CA, and IIS were placed on the domain controllers, shares were created on the `C:` drive, and a `.local` domain name was used[cite: 1]. In a production environment, these would be replaced with separate role servers, dedicated data drives, and a registered subdomain[cite: 1].

## Detailed Documentation
Step-by-step instructions and test results with screenshots are included in the attached PDF or DOCX documentation[cite: 1].
