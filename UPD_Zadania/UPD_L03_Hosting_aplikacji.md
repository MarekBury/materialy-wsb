# Zadania

**Nawiasy ostre we wszystkich poleceniach należy zamienić na nazwy własne.**

## 1. Instalacja Docker Community Edition

**Krok 1.** Utwórz nową maszynę wirtualną z systemem operacyjnym Ubuntu Server a następnie zaloguj się do niej za pomocą SSH (z Cloud Shell) albo za pomocą konsoli szeregowej w portalu Azure.

**Krok 2.** Na nowo utworzonej maszynie zainstaluj Docker Engine według poniższej instrukcji.

1. Zaktualizuj indeks pakietu `apt`.

```sh
sudo apt update
```

2. Zainstaluj pakiety umożliwiające `apt` korzystanie z repozytoriów przez HTTPS.

```sh
sudo apt install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  software-properties-common
```

3. Dodaj oficjalny klucz GPG Dockera.

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

4. Skonfiguruj repozytorium ze stabilnymi wersjami Dockera.

```sh
sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"
```

5. Zaktualizuj ponownie indeks pakietu `apt`.

```sh
sudo apt update
```

6. Zainstaluj najnowsze wersje Docker Engine Community Edition i containerd.

```sh
sudo apt install docker-ce docker-ce-cli containerd.io
```

7. Utwórz grupę użytkowników `docker`.

```sh
sudo groupadd docker
```

8. Dodaj do grupy `docker` bieżącego użytkownika.

```sh
sudo usermod -aG docker $USER
```

9. Wyloguj się a następnie zaloguj ponownie.

10. Sprawdź czy instalacja przebiegła pomyślnie uruchamiając testowy kontener.

```sh
docker run hello-world
```

11. Wyświetl listę uruchomionych kontenerów. Na liście powinien znaleźć się kontener `hello-worl`.

```sh
docker ps -a
```

12. Usuń kontener `hello-world` podając jego identyfikator uzyskany w poprzednim poleceniu.

```sh
docker rm <container-id>
```

13. Usuń obraz `hello-world`.

```sh
docker rmi hello-world
```

## 2. Tworzenie obrazów i uruchamianie kontenerów Docker lokalnie

**Krok 1.** Na utworzonej maszynie wirtualnej zainstaluj system kontroli wersji git.

```sh
sudo apt install git
```

**Krok 2.** Pobierz repozytorium z GitHub z kodem źródłowym aplikacji napisanej w Node.js.

```sh
git clone https://github.com/d-sobczyk/docker-nodejs-express-app
```

**Krok 3.** Przejdź do katalogu z aplikacją i zbuduj obraz Docker.

```sh
docker build -t <user-name>/<repository-name> .
```

**Krok 4.** Otwórz port `80` w ustawieniach firewalla maszyny wirtualnej.

**Krok 5.** Uruchom kontener Docker na maszynie wirtualnej a następnie w przeglądarce internetowej przejdź pod adres IP serwera wirtualnego. Jeżeli otworzy się strona z napisem "Express" oznacza to, że aplikacja została uruchomiona poprawnie.

```sh
docker run -p80:3000 <user-name>/<repository-name>
```

## 3. Uruchamianie kontenerów w usłudze Azure Container Instances

**Krok 1.** Zaloguj się do portalu Azure i otwórz Cloud Shell.

**Krok 2.** Utwórz nową grupę zasobów na potrzeby zadania o dowolnej nazwie.

**Krok 3.** Utwórz kontener na bazie obrazu `mcr.microsoft.com/azuredocs/aci-helloworld`.

```sh
az container create \
  --resource-group <resource-group-name> \
  --name <container-name> \
  --image mcr.microsoft.com/azuredocs/aci-helloworld \
  --dns-name-label <label> \
  --ports 80
```

**Krok 4.** Sprawdź status kontenera i pobierz jego adres.

```sh
az container show \
  --resource-group <resource-group-name> \
  --name \
  --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
  --output table
```

**Krok 5.** Otwórz przeglądarkę i przejdź pod adres znajdujący się w kolumnie FQDN w tabeli z poprzedniego polecania. Jeżeli wyświetliła się strona z napisem "Welcome to Azure Container Instances" oznacza to, że kontener został poprawnie uruchomiony.

**Krok 6.** Wyświetl logi aplikacji uruchomionej w kontenerze.

```sh
az container logs \
  --resource-group <resource-group-name> \
  --name <container-name>
```

**Krok 7.** Usuń kontener.

```sh
az container delete \
  --resource-group <resource-group-name> \
  --name <container-name>
```

**Krok 8.** Usuń grupę zasobów.

```sh
az group delete \
  --name <resource-group-name>
```

## 4. Zapisywanie obrazów w usłudze Azure Container Registry

**Krok 1.** Zainstaluj narzędzia Azure CLI na maszynie wirtualnej.

```sh
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

**Krok 4.** Zaloguj się do Azure za pomocą Azure CLI.

```sh
az login
```

**Krok 5.** Utwórz instancję Azure Container Registry i zapisz w niej obraz.

1. Utwórz instancję Azure Container Registry.

```sh
az acr create \
  --resource-group <resource-group-name> \
  --name <aci-name> \
  --sku Basic
```

2. Zaloguj się do rejestru.

```sh
az acr login \
  --name <aci-name>
```

3. Oznacz obraz Docker utworzony w zadaniu 2.

```sh
docker tag <user-name>/<repository-name> <aci-name>.azurecr.io/<repository-name>
```

4. Wypchnij obraz do rejestru.

```sh
docker push <aci-name>.azurecr.io/<repository-name>
```

5. Usuń obraz z maszyny wirtualnej.

```sh
docker rmi <aci-name>.azurecr.io/<repository-name>
```

**Krok 6.** Usuń instancję Azure Container Registry.

```sh
az acr delete \
  --resource-group <resource-group-name>
  --name <acr-name>
```

**Krok 7.** Usuń grupę zasobów.

```sh
az group delete \
  --name <resource-group-name>
```