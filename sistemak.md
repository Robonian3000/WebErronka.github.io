# 3. Sistema Eragileen Ezartzea (SISA 1 — 1. Erronka)

**Egilea:** Iñigo (4. Taldea)  
**Zerbitzariaren Izena (Hostname):** `Artaburu`  
**Sistema Eragilea:** Windows Server  
**IP Helbidea:** `192.168.70.9`  
**Web Zerbitzaria:** Internet Information Services (IIS) — RAID 5 biltegiratzean kokatua  

---

## 1. Sistema Eragileen Oinarri Teorikoak

### 1.1. Sistema Eragileen Funtzioak
Sistema eragilea (SE) ordenagailu baten hardwarearen eta erabiltzailearen (zein aplikazioen) arteko bitartekaria da. Bere funtzio nagusiak hauek dira:

* **Prozesuen kudeaketa (CPU):** Prozesuen sorrera, exekuzioa eta amaiera kontrolatzen ditu. CPUaren denbora banatzen du algoritmoen bidez, ataza anitzeko funtzionamendua ahalbidetuz.
* **Memoria nagusiaren kudeaketa (RAM):** Programa bakoitzari behar duen memoria-espazioa esleitzen eta askatzen dio. Memoria birtuala kudeatzen du eta prozesu batek beste baten memoria ez inbaditzea bermatzen du.
* **Sarrera/Irteerako (S/I) kudeaketa:** Periferikoen eta biltegiratze-gailuen komunikazioa kudeatzen du, kontroladoreen (driver-en) eta bufferren bitartez.
* **Fitxategi-sistemaren kudeaketa:** Informazioa era ordenatu eta hierarkikoan antolatzen du diskoan (fitxategiak, karpetak eta baimenak), gure kasuan NTFS fitxategi-sistema erabiliz.
* **Segurtasuna eta babesa:** Erabiltzaileen autentifikazioa kontrolatzen du, baimenak kudeatzen ditu eta baimenik gabeko sarbideak saihesten ditu.
* **Erabiltzaile-interfazea eskaintzea:** Erabiltzaileari sistemarekin komunikatzeko bidea ematen dio, bai interfaze grafikoaren bidez (GUI) bai komando-lerroaren bidez (CLI/PowerShell).

### 1.2. Sistema Eragileen Osagaiak
Sistema eragile baten barne-arkitektura elkarrekin koordinatutako hainbat moduluz osatuta dago:

* **Nukleoa (Kernel):** Sistemaren bihotza da. Hardwarearen gainean zuzenean kokatzen da eta modu pribilegiatuan (*kernel mode*) exekutatzen da. Memoria, prozesuak eta hardwarearen atzipena kontrolatzen ditu.
* **Sistema-deiak (System Calls / API):** Aplikazioen eta nukleoaren arteko zubi edo interfazea da. Programa batek hardware-baliabide bat eskatzeko erabiltzen ditu.
* **Kontroladoreak (Gailu-gidariak / Drivers):** Sistema eragileari hardware zehatz batekin (sare-txartela, disko kontrolatzailea...) komunikatzeko aukera ematen dioten software-zatiak dira.
* **Agindu-interpretea (Shell):** Erabiltzaileak aginduak sartzeko erabiltzen duen geruza (GUI grafikoa edo CLI kontsola).
* **Fitxategi-sistema (File System):** Disko gogorretan datuak fisikoki egituratzeko, bilatzeko eta gordetzeko egitura logikoa (NTFS).
* **Sistemaren zerbitzuak (Services):** Atzeko planoan etengabe martxan dauden prozesuak (gure kasuan, IIS web zerbitzaria).

---

## 2. IE1: Sistema Eragilearen Instalazioa, Sarea eta Eguneraketak

### 2.1. Makina Birtuala, Ekipoaren Izena eta Sarearen Konfigurazioa
Proiekturako **VMware Workstation** birtualizazio-ingurunea erabili da. Sare-komunikazioa bermatzeko, sare-moldagailua **Bridged (Zubi)** moduan konfiguratu da, fisikoki konektatutako Realtek sare-txartelari zuzenean lotuz.

![Sare Moldagailua](./Argazkiak%20Sistemas/1%20-%20Adaptadorea%20aldatu.png)

Rúbrikaren jarraibideak jarraituz, ekipoari **`Artaburu`** izen ofiziala ezarri zaio eta IP helbideratze estatikoa konfiguratu da:
* **Ekipoaren izena (Hostname):** `Artaburu`
* **IP helbidea:** `192.168.70.9`
* **Azpisare-maskara:** `255.255.255.0`
* **Atebidea (Gateway):** `192.168.70.1`

| Ekipoaren Izena (Artaburu) | IP Konfigurazioa |
| :---: | :---: |
| ![Ekipoaren Izena](./Argazkiak%20Sistemas/1.5%20-%20Ekipoaren%20izena.png) | ![IP Konfigurazioa](./Argazkiak%20Sistemas/2%20-%20IP%20konfig.png) |

### 2.2. Disko Gehigarriak Gehitzea (GPT)
Sistemaren biltegiratze-ahalmena eta RAID egiturak prestatu ahal izateko, makinari **10 GB-ko disko birtual gehigarriak** gehitu zaizkio SCSI kontrolatzailearen bidez, eta guztiak **GPT (GUID Partition Table)** partizio-taula modernoan hasieratu dira.

![Diskoak Gehitu](./Argazkiak%20Sistemas/4%20-%20Diskoak%20raidak%20egiteko.png)

### 2.3. Eguneraketak eta Berreskuratze-teknikak (Snapshot)
Sistemaren osotasuna eta segurtasuna bermatzeko:
1. **Windows Update:** Eguneraketa guztiak egiaztatu eta instalatu dira.
2. **Instantánea (Snapshot):** VMware bidez berreskuratze-puntu garbi bat sortu da (*Punto Limpio - Sistema eta Diskoak Preparados*), akatsik balego sistema egoera egonkorrera berehala itzuli ahal izateko.

| Windows Update Eguneraketak | Berreskuratze Snapshot-a |
| :---: | :---: |
| ![Windows Eguneratu](./Argazkiak%20Sistemas/5%20-%20Windows%20Eguneratu.png) | ![Snapshot](./Argazkiak%20Sistemas/7%20-%20Spanshot-a.png) |

---

## 3. IE2: Oinarrizko Softwarea, Segurtasun Politikak eta IIS Web Zerbitzaria

### 3.1. Segurtasun Politikak: Pasahitzak eta Kontuen Blokeoa (`secpol.msc`)
Enpresaren segurtasun-eskakizunak betetzeko, **Tokiko Segurtasun Zuzentarauak (Local Security Policy)** aplikatu dira:
* **Pasahitzen konplexutasuna:** Gaituta (maiuskulak, minuskulak, zenbakiak eta karaktere bereziak).
* **Gutxieneko luzera:** 8 karaktere.
* **Pasahitzen historia:** 5 pasahitz gogoratu (errepikapenak saihesteko).
* **Kontuen blokeoa:** **3 saiakera okerren ostean kontua automatikoki blokeatzea** (indarkeria gordinaren bidezko erasoak saihesteko).

| Pasahitz Politikak | Kontuen Blokeo Politika |
| :---: | :---: |
| ![Pasahitz Politikak](./Argazkiak%20Sistemas/9%20-%20Pasahitz%20Politikak.png) | ![Kontuen Blokeoa](./Argazkiak%20Sistemas/10%20-%20Kontuaren%20Blokeoa.png) |

> 🔑 **Sareak Modulurako Pasahitza:** Administratzaile-konturako ezarri den pasahitza **`Admin123@`** da. Pasahitz hau Sareak moduluko kideari (Peru) eman zaio bere ASCII bihurketa eta **CRC-6-GSM** kontrol-polinomioaren kalkulua burutu dezan.

### 3.2. Erabiltzaileak eta Taldeak (`lusrmgr.msc`)
Rol hierarkikoak bereizteko bi talde eta dagozkien kontuak sortu dira:
* **`G_Teknikariak`:** Mantentze eta kudeaketarako baimen osoak dituzten erabiltzaileak (`admin_web`).
* **`G_Langileak`:** Enpresako langile arrunten profila (`langile1`), baimen mugatuekin.

![Taldeak eta Erabiltzaileak](./Argazkiak%20Sistemas/11%20-%20Taldeak%20eta%20Erabiltzaileak.png)

### 3.3. NTFS Baimenen Ezarpena Karpeta Partekatuan
Datu-bolumenean `Enpresa_Datuak` karpeta sortu da eta herentzia desgaitu ondoren NTFS baimen zehatzak ezarri dira:
* `G_Teknikariak`: **Kontrol Osoa (Full Control)**.
* `G_Langileak`: **Irakurketa eta Exekuzioa (Read & Execute)** soilik (ezin dute fitxategirik ezabatu ezta aldatu).

| NTFS Baimenak (Opciones avanzadas) | NTFS Baimenak Taldeka |
| :---: | :---: |
| ![NTFS 1](./Argazkiak%20Sistemas/12%20-%20NTFS%20Baimenak1.png) | ![NTFS 2](./Argazkiak%20Sistemas/13%20-%20NTFS%20Baimenak2.png) |

### 3.4. Web Zerbitzaria (IIS) eta RAID 5-era Lotura
1. **IIS (Internet Information Services)** rola instalatu da zerbitzarian.
2. Webgunearen karpeta nagusia (`Default Web Site`) disko lokaletik (`C:\`) **RAID 5 bolumenera (`H:\Web_Enpresa`)** bideratu da. Horrela, web-orria hardware-akatsen aurrean babestuta dago.
3. Sare lokaleko beste gailu batetik (portatila / mugikorra) `http://192.168.70.9` atzituz webgunea behar bezala zerbitzatzen dela frogatu da.

| IIS Rola Instalatuta | Bide Fisikoa RAID 5-ean |
| :---: | :---: |
| ![IIS Instalatu](./Argazkiak%20Sistemas/3%20-%20IIS%20instalatu.png) | ![Bide Fisikoa](./Argazkiak%20Sistemas/16%20-%20IIS%20RAID%20bide%20fisikoa.png) |

#### Froga Erreala Nabigatzailean:
![Webgunea Sarean](./Argazkiak%20Sistemas/17%20-%20Nabigatzailea%20IIS%20RAID5.png)

---

## 4. IE3: Biltegiratze Sistemak, Bolumenak eta RAID Egiturak

### 4.1. Bolumen Sinpleak, Banatuak (Spanned) eta Disko Kuotak
Diskoen kudeaketa aurreratua erakusteko, hainbat bolumen mota sortu dira:
* **Bolumen Sinplea (`E:`):** Disco 1 erabiliz.
* **Bolumen Banatua / Distribuido (`F:`):** Disco 2 eta Disco 3 (disko dinamikoak) elkartuz sortutako **20 GB-ko bolumen bateratua** (kolore mororea).
* **Disko-kuotak:** Erabiltzaile bakoitzak har dezakeen espazioa mugatzeko kuotak aktibatu dira:
  * Espazio-muga: **2 GB**.
  * Abisu-maila: **1,5 GB**.

| Bolumen Sinple eta Banatuak | Disko Kuoten Ezarpena |
| :---: | :---: |
| ![Bolumenak](./Argazkiak%20Sistemas/6%20-%20%20Bolumen%20Simple%20eta%20Banatuak.png) | ![Kuotak](./Argazkiak%20Sistemas/8%20-%20Disko%20Cuotak%20.png) |

### 4.2. Biltegiratze Erredundantea: RAID 1 eta RAID 5

#### A) RAID 1 — Ispilua (`G:`)
* **Diskoak:** Disco 4 eta Disco 5 (dinamikoak).
* **Ezaugarriak:** Informazio bikoiztua (ispilua / *mirror*). Diskoetako batek huts egingo balu, sistemak etenik gabe funtzionatzen jarraitzen du.
* **Kolorea:** Granate / Gorri iluna.

![RAID 1 Ispilua](./Argazkiak%20Sistemas/14%20-%20Raid%201.png)

#### B) RAID 5 — Paritatea (`H:`)
* **Diskoak:** Disco 6, Disco 7 eta Disco 8 (dinamikoak, 10 GB bakoitzak).
* **Ezaugarriak:** Datuak eta paritatea 3 diskoetan zehar banatzen dira. Erabilgarri dagoen espazioa guztira **20 GB** da ($N-1$). Disko batek huts eginez gero, paritatearen bidez datuak birkalkulatzen dira.
* **Aplikazio praktikoa:** Bolumen honetan kokatu da enpresaren **webgune ofiziala (`H:\Web_Enpresa`)**.
* **Kolorea:** Urdin argia / Ziana.

![RAID 5 Paritatea](./Argazkiak%20Sistemas/15%20-%20Raid%205.png)

### 4.3. Babeskopia Automatikoak (Windows Server Backup)
Sistemaren datuak babesteko eta hondamenen aurrean erantzuteko:
1. **Ezaugarriaren instalazioa:** *Copias de seguridad de Windows Server* instalatu da zerbitzarian.
2. **Kopia programatua:** Eguneroko babeskopia automatiko bat ezarri da gaueko orduetan, `Web_Enpresa` karpeta eta sistemaren egoera bolumen seguru batean gordetzeko.

| Windows Server Backup Ezaugarria | Babeskopiaren Programazioa |
| :---: | :---: |
| ![Kopien Rola](./Argazkiak%20Sistemas/18%20-%20Kopien%20rola%20instalatu.png) | ![Babeskopia](./Argazkiak%20Sistemas/19%20-%20Segurtasun%20kopia.png) |

---

## 5. Ondorioak eta Hurrengo Urratsak (2. Astea)

Lehen aste honetan (Urriak 19-23) planifikatutako helburu tekniko guztiak arrakastaz gainditu dira:
* Zerbitzariaren izena (**`Artaburu`**) eta sarea konfiguratuta.
* Erabiltzaile eta baimen-hierarkia operatiboa.
* Biltegiratze-arkitektura osoa (sinplea, banatua, RAID 1 eta RAID 5) martxan.
* IIS zerbitzaria saretik atzigarri RAID 5 bolumenean.
* Babeskopia-plan automatizatua ezarrita Windows Server Backup bidez.

**2. Asteko Zereginak (Urriak 26-30):**
1. Hirugarrenen tresnekin (adib. Cobian Reflector) babeskopia gehigarriak konfiguratzea.
2. Disko-irudiak sortzea eta hondamenen aurreko berreskuratze-plana garatzea.
3. Azken integrazioa eta proiektuaren aurkezpena.
