# Dokumentacija za Guestbook app
_Tehnologije i alati u projektu: **NodeJS**, **MongoDB**, **NGINX**, **Azure Cloud, Ubuntu Server VM**, **DuckDNS**_
# Koraci deployment-a
## Kreiranje Ubuntu Server VM na Azure Cloud platformi
![Azure cloud VM setup](https://i.gyazo.com/1dba7e74b3fc81071532974be08df754.png)
![](https://i.gyazo.com/0fd7d111ce06ca5d483309175207f929.png)
## Pristup kreiranoj masini
![SSH to VM over Terminal](https://i.gyazo.com/b8de0688e5b378b55f5b99e76e533da4.png)
## Azuriranje i priprema potrebnog softvera
* `sudo apt update && sudo apt upgrade -y`
* NodeJS install
```
# Download and install fnm:
curl -o- https://fnm.vercel.app/install | bash

# Download and install Node.js:
fnm install 22

# Verify the Node.js version:
node -v # Should print "v22.14.0".

# Verify npm version:
npm -v # Should print "10.9.2".

```
![NodeJS success](https://i.gyazo.com/39f3979352b91401d5a046484fb7a895.png)
* MongoDB install
    * https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/

![MongoDB success](https://i.gyazo.com/98fb5c0b2bbbbfc406fae0828433f2a6.png)

* Nginx install
`sudo apt install nginx`

## Podesavanje Guestbook app (baza podataka, node server, nginx host, DNS)
### Node init i npm potrebni paketi
`npm init -y`
`npm install express monogoose socket.io dotenv`
![](https://i.gyazo.com/58e828bde8c9eec749bf5bf66453fc16.png)

### MongoDB baza podataka
`mongosh`
`> use guestbook`
`guestbook> db.createCollection("messages")`
`guestbook> db.messages.insertOne([{ message: "Hello from MongoDB!" }])`

### NodeJS server i sama aplikacija

![](https://i.gyazo.com/a8c3cab9a142b332f06e9c86a4903a66.png)
PM2 za aktivaciju servera u pozadini i na startup-u sistema
`npm install -g pm2`

### NGINX active site setup

DuckDNS konfiguracija

NGINX config sa DNS serverom.
`sudo nano /etc/nginx/sites-available/guestbook`
`sudo ln -s /etc/nginx/sites-available/guestbook /etc/nginx/sites-enabled/`

## Pokretanje aplikacije i testiranje
`pm2 start server.js --name guestbook`
`pm2 startup`
`pm2 save`

Otvaranje portova
`sudo ufw allow 22,80,443`
`sudo ufw enable`

![](https://i.gyazo.com/6c0f2a23218d3140a341e8c6aae50812.png)
Slika app <

## HTTPS sertifikacija
