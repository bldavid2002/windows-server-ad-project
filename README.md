# Windows Server és Kliens Integrációs Labor

## Projekt Összefoglaló
A projekt célja egy izolált laboratóriumi környezet létrehozása VirtualBoxban, amely egy vállalati hálózat működését szimulálja. A fókusz az Active Directory (AD DS) szolgáltatás felépítésén, a felhasználókezelésen és a központi csoportházirendek (GPO) konfigurálásán van.

## Alkalmazott Technológiák és Környezet
* **Virtualizáció:** Oracle VirtualBox
* **Szerver OS:** Windows Server 2022 (Domain Controller, DNS)
* **Kliens OS:** Windows 11 Enterprise
* **Hálózati beállítás:** Internal Network / NAT Network

## Főbb Konfigurációs Lépések
1. **Szerver Alapbeállítások:** Statikus IP cím (10.0.2.10) és gépnév (SRV-DC01) beállítása.
2. **Active Directory Telepítés:** AD DS szerepkör telepítése és új Forest (`DomCont.local`) létrehozása.
3. **Szervezeti Struktúra (OU):** Dedikált konténerek (IT_Department, HR_Department) és tesztfelhasználók (pl. Balogh Dávid) létrehozása.
4. **Group Policy (GPO) és Fájlmegosztás:** Központi `Common_Drive_Map` házirend kialakítása, amely bejelentkezéskor automatikusan felcsatolja a közös meghajtót (Z: drive) a kliensek számára a `\\SRV-DC01\CompanyData` útvonalról.
5. **Kliens Integráció:** A Windows 11 kliens gép sikeres csatlakoztatása a tartományhoz és a GPO szabályok érvényesülésének tesztelése.

## Részletes Dokumentáció
A konfiguráció lépésről lépésre történő leírása és a tesztelési eredmények (képernyőképekkel) a mellékelt PDF dokumentumban találhatók.


# Windows Server & Client Integration Lab

## Project Overview
The goal of this project is to create an isolated laboratory environment in VirtualBox that simulates the operation of a corporate network. The focus is on building the Active Directory (AD DS) service, user management, and configuring centralized Group Policies (GPO).

## Technologies & Environment Used
* **Virtualization:** Oracle VirtualBox
* **Server OS:** Windows Server 2022 (Domain Controller, DNS)
* **Client OS:** Windows 11 Enterprise
* **Network Settings:** Internal Network / NAT Network

## Key Configuration Steps
1. **Basic Server Configuration:** Setting up a static IP address (10.0.2.10) and hostname (SRV-DC01).
2. **Active Directory Installation:** Installing the AD DS role and creating a new Forest (`DomCont.local`).
3. **Organizational Structure (OU):** Creating dedicated containers (IT_Department, HR_Department) and test users (e.g., Dávid Balogh).
4. **Group Policy (GPO) & File Sharing:** Establishing a central `Common_Drive_Map` policy that automatically mounts the shared drive (Z: drive) for clients upon login from the `\\SRV-DC01\CompanyData` path.
5. **Client Integration:** Successfully joining the Windows 11 client machine to the domain and verifying the application of GPO rules.

## Detailed Documentation
A step-by-step guide to the configuration and the test results (including screenshots) can be found in the attached PDF document.
