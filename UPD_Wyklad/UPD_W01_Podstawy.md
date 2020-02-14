---
marp: true
theme: custom
paginate: true
header: Wprowadzenie do chmury obliczeniowej i platformy Azure
footer: © 2019 Dariusz Sobczyk
---

# Podstawy przetwarzania w chmurze
Wprowadzenie do chmury obliczeniowej i platformy Azure.

---

**Chmura obliczeniowa** jest to platforma dostarczająca usługi obliczeniowe, takie jak serwery, magazyny, bazy danych, sieci oraz inne, za pośrednictwem Internetu.

---

## Korzyści z używania chmury obliczeniowej
* Zwiększona kontrola nad kostami
* Globalna skala
* Bezpieczeństwo
* Elastyczność zarządzania
* Niezawodność

---

## Rodzaje chmur obliczeniowych
* Publiczna
* Prywatna
* Hybrydowa

---

## Rodzaje usług w chmurze obliczeniowej
* IaaS - Infrastruktura jako usługa (ang. Infrastructure as a Service)
* PaaS - Platforma jako usługa (ang. Platform as a Service)
* FaaS - Funkcja jako usługa (ang. Function as a Service) lub Serverless
* SaaS - Oprogramowanie jako usługa (ang. Software as a Service)

---

## Zastosowania chmury obliczeniowej
* Tworzenie aplikacji
* Przechowywanie danych
* Przesyłanie strumieniowe
* Dostarczanie oprogramowania
* Analiza danych

---

## Wiodący dostawcy chmur obliczeniowych
* Amazon
* Microsoft
* Google
* Alibaba
* IBM
* Oracle

---

## Usługi platformy Azure

---

## Infrastruktura
* Maszyny wirtualne
* Magazyny
* Sieci

---

## Hostowanie aplikacji
* Azure App Service
* Azure Functions
* Azure Batch
* Azure Containers
* Azure Kubernetes Service

---

## Wydajność aplikacji
* Azure Front Door
* Azure Traffic Manager
* Azure CDN
* Azure Redis Cache

---

## Magazynowanie danych
* Azure Storage
* Azure Cosmos DB
* Azure SQL Database
* Azure for MySQL
* Azure for PostgreSQL
* Azure for MariaDB

---

## Zabezpieczanie aplikacji
* Azure Active Directory
* Microsoft Identity Platform
* Azure Backup
* Azure Encryption
* Azure Key Vault
* Azure API Management

---

## Monitorowanie aplikacji
* Azure Log Analytics
* Azure Monitor
* Azure Application Insights

---

## Maszyny wirtualne na platformie Azure
* Wysoka skalowalność
* Szyfrowanie danych
* Bezpieczeństwo sieci
* Windows lub Linux
* Płatność zgodnie z rzeczywistym użyciem

---

## Zarządzanie maszynami wirtualnym w Azure
* Azure Portal
* Azure CLI (Windows, Linux, MacOS)
* Azure SDK (.NET, Java, JavaScript, Python)

Polecenie `az vm` z pakietu Azure CLI służy do zarządzania maszynami wirtualnymi.

---

```sh
# Tworzy nową maszynę wirtualną 'vm' w grupie zasobów 'vm-rg'.
az vm create \
  --resource-group vm-rg \
  --name vm \
  --size Standard_DS1 \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

---

```sh
# Wyświetla listę dostępnych lokalizacji dla konta.
az account list-locations \
  --output table
```

---

```sh
# Wyświetla adres IP maszyny wirtualnej 'vm' z grupy zasobów 'vm-rg'
az vm list-ip-addresses \
  --resource-group vm-rg \
  --name vm \
  --output table
```

---

```sh
# Wyświetla listę dostępnych obrazów.
az vm image list \
  --output table
```

