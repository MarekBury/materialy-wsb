---
marp: true
theme: custom
paginate: true
header: Wprowadzenie do usługi Azure Functions
footer: © 2019 Dariusz Sobczyk
---

# Hosting aplikacji
Wprowadzenie do usługi Azure Functions.

---

**Azure Functions** to usługa przetwarzania bezserwerowego (ang. serverless) platformy Azure, umożliwiająca uruchamianie małych fragmentów kodu zwanych funkcjami.

**Przetwarzanie bezserwerowe** jest to model wykonania, w którym dostawca usługi odpowiada za uruchamianie kodu poprzez dynamiczną alokację potrzebnych zasobów na czas jego działania.

---

## Zalety wykorzystania
* Utrzymanie infrastruktury spoczywa na dostawcy usługi
* Automatyczne skalowanie zasobów obliczeniowych
* Zwiększona kontrola nad kosztami utrzymania

---

## Najważniejsze cechy
* Wsparcie dla języków C#, Java, JavaScript, TypeScript, Python, PowerShell
* Płatność za czas działania kodu
* Wsparcie dla ciągłego wdrażania (ang. continuous deployment)
* Możliwość pisania kodu w Azure Portal
* Środowisko uruchomieniowe jest oprogramowaniem typu open-source

---

## Uruchamianie kodu
* **HTTPTrigger** - funkcja wyzwolona na żądanie HTTP
* **TimerTrigger** - funkcja wyzwolona zgodnie z harmonogramem
* **CosmosDBTrigger** - funkcja wyzwolona na modyfikację w bazie danych
* **BlobTigger** - funkcja wyswolona na dodanie obiektu typu blob
* **QueueTrigger** - funkcja wyzwolona na komunikat dodany do kolejki

---

## Plany cenowe
* **Plan zużycia** - płatność za użyte zasoby podczas działania kodu
* **Plan usługi App Service** - uruchamianie kodu w usłudze App Service

---

**Azure Functions Core Tools** - zestaw narzędzi pozwalających na tworzenie, testowanie i wdrażanie funkcji w środowisku lokalnym.

---

### Tworzenie projektu

```sh
# Tworzy nowy projekt 'func-app' lokalnie.
func init func-app \
  --language javascript \
  --worker-runtime node
```

---

### Tworzenie funkcji

```sh
# Tworzy nową funkcję 'http-func' w lokalnym projekcie 'func-app'
cd func-app && func new \
  --name http-func \
  --template "HttpTrigger"
```

---

### Uruchamianie aplikacji

```sh
# Uruchamia aplikację lokalnie.
func start

# Wywołuje funkcję z dodatkowym argumentem.
curl http://localhost:7071/api/http-func?name=X

# Na wyjściu 'Hello X'
```

---

## Zarządzanie usługą z poziomu Azure CLI

```sh
# Wyświetla pomoc dotyczącą aplikacji funkcji
az functionapp --help
```

---

### Tworzenie konta magazynu

```sh
# Tworzy nowe konto magazynu 'funcappstorage' w grupie zasobów 'func-app-rg'.
az storage account create \
  --name funcappstorage \
  --location westeurope \
  --resource-group func-app-rg \
  --sku Standard_LRS
```

---

### Tworzenie aplikacji funkcji

```sh
# Tworzy nową aplikację funkcji 'func-app' w grupie zasobów 'func-app-rg'
az functionapp create \
  --resource-group func-app-rg \
  --consumption-plan-location westeurope \
  --name func-app \
  --storage-account funcappstoragewsb \
  --runtime node
```

---

### Wdrożenie aplikacji funkcji

```sh
# Wdraża lokalnie utworzony projekt.
func azure functionapp publish func-app
```

### Usunięcie aplikacji funkcji

```sh
# Usuwa aplikację funkcji.
az functionapp delete \
  --name func-app \
  --resource-group func-app-rg
```