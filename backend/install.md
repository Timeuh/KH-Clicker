# KH-Clicker : installation de l'API (dev)

## Prérequis

- Docker + Docker Compose
- .NET SDK 10 (pour l'IDE, IntelliSense, `dotnet build`)
- VS Code avec l'extension **C# Dev Kit**

## Installer le SDK .NET 10

```bash
sudo apt update
sudo apt install -y dotnet-sdk-10.0

dotnet --version
```

> Si le paquet est introuvable, installer via le script officiel :
> ```bash
> curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel 10.0
> ```

## VS Code

Installer l'extension **C# Dev Kit** (`ms-dotnettools.csdevkit`).


## Lancer l'API

```bash
docker compose -f docker-compose-dev.yml up --build
```

- API : http://localhost:4000
- Adminer : http://localhost:8080
- Hot reload actif (`dotnet watch`) : le code de `./backend` est monté dans le conteneur.

## Commandes utiles

```bash
docker compose -f docker-compose-dev.yml logs -f backend
docker compose -f docker-compose-dev.yml down
docker compose -f docker-compose-dev.yml down -v