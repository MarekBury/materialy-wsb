---
marp: true
theme: custom
paginate: true
header: Programowanie i architektura aplikacji w chmurze
footer: © 2020 Dariusz Sobczyk
---

# Hosting aplikacji w usłudze Azure App Service

---

**App Service** jest to usługa oparta na protokole HTTP umożliwiająca hostowanie stron i aplikacji internetowych, interfejsów programowania (REST API) oraz zapleczy dla aplikacji mobilnych (ang. mobile back-ends).

---

## Zalety wykorzystania
* Utrzymanie infrastruktury spoczywa na dostawcy usługi
* Automatyczne skalowanie zasobów obliczeniowych
* Zwiększona kontrola nad kosztami utrzymania

---

## Najważniejsze cechy
* Wsparcie dla technologii .NET, Java, Node.js, PHP, Ruby, Python
* Wsparcie dla ciągłego wdrażania (ang. continuous deployment)
* Globalna skala i gwarantowane SLA na poziomie 99,95%
* Możliwość użycia niestandardowych domen oraz włączenia protokołu HTTPS
* Uwierzytelnianie Azure Active Directory lub Google, Facebook, Twitter i Microsoft
* Integracja z IDE Visual Studio i edytorem Visual Studio Code
* Wsparcie dla tworzenia i udostępniania interfejsów RESTful
* Uruchamianie kodu bezserwerowego (ang. serverless)

---

**App Service Plan** jest to plan usługi w ramach którego działają aplikacje (Web Apps). Plan usługi definiuje zestaw zasobów obliczeniowych do uruchamiania aplikacji.

---

## Plany usługi App Service
* Współdzielone (Free, Shared)
* Dedykowane (Basic, Standard, Premium, PremiumV2)
* Izolowane (Isolated)

---

### Tworzenie planu

```sh
az appservice plan create \
  --resource-group <nazwa-grupy-zasobów> \
  --name <nazwa-planu> \
  --location <lokalizacja> \
  --sku <warstwa-cenowa>
```

---

### Wyświetlanie listy dostępnych lokalizacji

```sh
az appservice list-locations \
  --sku <warstwa-cenowa>
```

---

### Wyświetlanie dostępnych warstw cenowych

```sh
az appservice list-locations --help
```

---

### Skalowanie planu (poziome)

```sh
az appservice plan update \
  --resource-group <nazwa-grupy-zasobów> \
  --name <nazwa-planu> \
  --number-of-workers <liczba-wystąpień>
```

---

### Skalowanie planu (pionowe)

```sh
az appservice-plan update \
  --resource-group <nazwa-grupy-zasobów> \
  --name <nazwa-planu> \
  --sku <warstwa-cenowa>
```

---

### Usuwanie planu

```sh
az appservice plan delete \
  --resource-group <nazwa-grupy-zasobów> \
  --name <nazwa-planu>
```

---

### Tworzenie aplikacji

```sh
az webapp create \
  --resource-group <nazwa-grupy-zasobów> \
  --plan <nazwa-planu> \
  --name <nazwa-aplikacji> \
  --runtime <środowisko-uruchomieniowe>
```

---

### Wyświetlanie listy środowisk uruchomieniowych

```sh
az webapp list-runtimes
```

---

## Wdrażanie kodu
* ZIP
* FTP
* Cloud sync (Dropbox, OneDrive)
* Local Git
* GitHub, BitBucket, Azure Repos
* GitHub Actions

---

### Wdrażanie kodu przez Git

```sh
az webapp deployment user set \
  --user-name <nazwa-użytkownika> \
  --password <hasło>
```

---

### Wdrażanie kodu przez Git (c.d.)

```sh
url=$(az webapp deployment source config-local-git \
  --resource-group <nazwa-grupy-zasobów> \
  --name <nazwa-aplikacji> \
  --query url \
  --output tsv)
```

---

### Wdrażanie kodu przez Git (c.d.)

```sh
git remote add azure $url
```

---

### Wdrażanie kodu przez Git (c.d.)

```sh
git push azure master
```

---

**Kudu** jest usługą umożliwiającą wdrażanie aplikacji za pomocą systemu kontroli wersji Git. Udostępnia również API do zarządzania ustawieniami aplikacji, plikami, procesami, wersjami środowisk uruchomieniowych, web hooks i web jobs.

---

### Usuwanie aplikacji

```sh
az webapp delete \
  --resource-group <nazwa-grupy-zasobów> \
  --name <nazwa-aplikacji>
```

---

## Dodatkowe funkcje
* Monitorowanie (logi, metryki)
* Testwy wydajnościowe (ang. load testing)
* Tworzenie i przywracanie kopii zapasowych
* Klonowanie
* Przenoszenie między regionami
* Uruchamianie zadań w tle (WebJobs)