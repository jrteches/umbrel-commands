---
description: >-
  Comandos para administrar el nodo Umbrel bajo SSH. Utiles para revisar errores
  o para cuando no podemos entrar al entorno web.
---

# ☂️ Comandos para administrar Umbrel

## Antes de empezar

Esta página esta hecha por mi mismo y no tengo relación alguna con getumbrel. Simplemente me gusta el proyecto y uso este software para gestionar mi nodo. Esta lista de comando esta hecha en principio para mi, pero creo que le puede ser de utilidad a mas de unos.

Si quieres colaborar con otros comandos o información interesante adelante!

{% hint style="info" %}
Usar estos comandos bajo vuestra propia responsabilidad. Si tienes dudas contacta con el soporte técnico.
{% endhint %}

## Links oficiales

* Pagina web: [https://getumbrel.com](https://getumbrel.com)
* Chat oficial de soporte en: [https://t.me/getumbrel](https://t.me/getumbrel)
* Preguntas frequentes y soporte: [https://umbrelinfo.gitlab.io](https://umbrelinfo.gitlab.io)
* Twitter: [https://twitter.com/getumbrel](https://twitter.com/getumbrel)
* Github: [https://github.com/getumbrel/umbrel](https://github.com/getumbrel/umbrel)

## Conectar a tu nodo mediante SSH

```bash
ssh umbrel@umbrel.local
```

* Contraseña en version &gt; 0.3.3 es el mismo password que usar en el entorno web
* Contraseña en versiones &lt; 0.3.3 es: `moneyprintergobrrr`

## Generar un debug completo para compartir con el soporte técnico.

#### Version &lt; 0.3.4

```bash
haste() { a=$(cat); curl -X POST -s -d "$a" https://umbrel-paste.vercel.app/documents | awk -F '"' '{print "https://umbrel-paste.vercel.app/"$6}'; }
~/umbrel/scripts/debug | haste
```

#### Version &gt; 0.3.4

Opción 1

```bash
 ~/umbrel/scripts/debug --upload
```

Opción 2

```bash
upload() { a=$(cat); curl -X POST -s -d "$a" https://umbrel-paste.vercel.app/documents | awk -F '"' '{print "https://umbrel-paste.vercel.app/"$6}'; }
~/umbrel/scripts/debug | upload
```

## Ubicación de bitcoin-cli i lncli

```bash
~/umbrel/bin
```

## Encender y apagar solamente Umbrel

Detener todos los servicios y apps relacionadas con Umbrel. Ojo no devuelve output. Esperar a que vuelva el prompt

```bash
sudo systemctl stop umbrel-startup
```

Encender todos los servicios y apps relacionadas con Umbrel. No devuelve output, esperar al prompt.

```bash
sudo systemctl start umbrel-startup
```

Esperar unos cuantos minutos a que todas las aplicaciones se levanten. Cuantas mas apps tengamos mas tardara el proceso.

### Arrancar los servicios docker de Umbrel

```bash
~/umbrel/docker-compose up --detach --build --remove-orphans
```

En mi caso, lnd se apagaba solo y tenia que reiniciar todo el nodo para levantarlo. De esta forma solamente se inician aquellos servicios o contenedores que por algun motivo se han apagado.



## Gestionar apps \(Encender, parar, instalar y desinstalar\)

Desinstalar una app

```bash
sudo ~/umbrel/scripts/app uninstall nombre-app
```

Instalar una app

```bash
sudo ~/umbrel/scripts/app install nombre-app
```

Detener una app

```bash
sudo ~/umbrel/scripts/app stop nombre-app
```

Iniciar una app

```bash
sudo ~/umbrel/scripts/app start nombre-app
```

### Tabla Nombre-app

| Nombre de la App | nombre para el comando |
| :--- | :--- |
| BTC RPC Explorer | `btc-rpc-explorer` |
| Mempool | `mempool` |
| Ride the lightning | `ride-the-lightning` |
| Lightning terminal | `lightning-terminal` |
| Thunderhub | `thunderhub` |
| Lnbits | `lnbits` |
| Samourai | `samourai` |
| Specter Desktop | `specter-desktop` |
| Sphinx Relay | `sphinx-relay` |
| BTCPay Server | `btcpay-server` |

## Ver el log de Bitcoin en tiempo real

```bash
tail -f ~/umbrel/bitcoin/debug.log
```

## Ver el log de Lightning en tiempo real

```bash
tail -f umbrel/lnd/logs/bitcoin/mainnet/lnd.log
```

## Comandos para bitcoind

Muestra diferentes informaciones sobre nuestro nodo

```bash
./bitcoin-cli getblockchainnfo
```

Muestra información sobre la red

```bash
./bitcoin-cli getnetworkinfo
```

## Listado de procesos

```bash
top
```

o también

```bash
htop
```

## Deshabilitar el boot de Umbrel

```bash
sudo systemctl disable umbrel-startup && sudo systemctl disable umbrel-connection-details && sudo systemctl disable umbrel-external-storage && sudo reboot
```

## Habilitar el boot de Umbrel

```bash
sudo systemctl enable umbrel-startup && sudo systemctl enable umbrel-connection-details && sudo systemctl enable umbrel-external-storage && sudo reboot
```

## Comprobar y solucionar problemas en el sistema de ficheros

```bash
sudo fsck -y /dev/sda1
```



