---
marp: true
theme: custom
paginate: true
header: Wprowadzenie do usługi Azure App Service
footer: © 2019 Dariusz Sobczyk
---

# Hosting aplikacji
Wprowadzenie do usługi Azure App Service.

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

## Zarządzanie usługą z poziomu Azure CLI

```sh
# Wyświetla pomoc dotyczącą zarządzania planami usługi App Service
az appservice --help

# Wyświetla pomoc dotyczącą zarządzania aplikacjami usługi App Service
az webapp --help
```

---

**App Service Plan** jest to plan usługi w ramach którego działają aplikacje (Web Apps). Plan usługi definiuje zestaw zasobów obliczeniowych do uruchamiania aplikacji.

---

### Tworzenie aplikacji

```sh
# Tworzy nowy plan 'webapp-plan' w grupie zasobów 'webapp-rg'
az appservice plan create \
  --name webapp-plan \
  --resource-group webapp-rg \
  --sku FREE

# Tworzy nową aplikację 'webapp-static` w planie `webapp-plan`
az webapp create \
  --name webapp-static \
  --resource-group webapp-rg \
  --plan webapp-plan
```

---

### Wdrażanie kodu przez FTP

```sh
# Pobiera dane uwierzytelniające FTP
creds=($(az webapp deployment list-publishing-profiles \
  --name webapp-static \
  --resource-group webapp-rg \
  --query "[?contains(publishMethod, 'FTP')].[publishUrl,userName,userPWD]" \
  --output tsv))

# Wgrywa plik index.html na serwer FTP
curl -T index.html -u ${creds[1]}:${creds[2]} ${creds[0]}/

# Strona dostępna jest pod adresem http://webapp-static.azurewebsites.net
curl http://webapp-static.azurewebsites.net
```

---

**Kudu** jest usługą umożliwiającą wdrażanie aplikacji za pomocą systemu kontroli wersji git. Udostępnia również API do zarządzania ustawieniami aplikacji, plikami, procesami, wersjami środowisk uruchomieniowych, web hooks i web jobs.

---

### Wdrażanie kodu przez GIT

```sh
# Ustawia dane uwierzytelniające na poziomie konta
az webapp deployment user set \
  --user-name webapp-user \
  --password webapp-password-123

# Pobiera adres URL zdalnego repozytorium git aplikacji
url=$(az webapp deployment source config-local-git \
  --name webapp-static \
  --resource-group webapp-rg \
  --query url \
  --output tsv)

# Dodaje URL zdalnego repozytorium git aplikacji.
git remote add azure $url

# Wysyła kod do zdalnego repozytorium git aplikacji.
git push azure master

# Strona dostępna jest pod adresem http://webapp-static.azurewebsites.net
curl http://webapp-static.azurewebsites.net
```

---

**Skalowanie pionowe** zwane również skalowaniem w górę lub w dół (ang. scale up, scale down), oznacza zmianę wydajności zasobu, np. zwiększenie rozmiaru maszyny wirtualnej.

**Skalowanie poziome** zwane również skalowaniem wszerz (ang. scale in, scale out), oznacza dodanie lub usunięcie zasobów, np. dodanie nowej maszyny wirtualnej.

---

### Skalowanie aplikacji

```sh
# Skaluje aplikację do dwóch instancji.
az appservice plan update \
  --number-of-workers 2 \
  --name webapp-plan \
  --resource-group webapp-rg
```

---

**Autoskalowanie** to proces dynamicznego przydzielania zasobów w zależności od obciążenia, np. dodanie nowych maszyn wirtualnych celem obsługi rosnącej liczby żądań.

---

### Usunięcie aplikacji

```sh
# Usuwa aplikację usługi App Service
az webapp delete \
  --name webapp-static \
  --resource-group webapp-rg

# Usuwa plan usługi App Service
az appservice plan delete \
  --name webapp-plan \
  --resource-group webapp-rg
```