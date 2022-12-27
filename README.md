# Micro Téléinfo V3

Dongle USB permettant de récupérer les trames téléinformation

Suivez mes nouveautés et autres projets sur mon [blog][4] 

## Historique


**New V3.0**

- Remplacement R3 par un potentiomètre
- Led séparées RX / TX

**Prototype V3.0**

- Réduction taille PCB
- Ajout LED information signal TIC
- Test Led Rouge/Vert pour RX/TX
- Test avec connecteur USB PTH/SMD
- Ajustement résistance TIC 220 Ohm
- Ajustement résistance MOSFET 3.3KOhm
- Changement chip FT230XS par CH9106

**V2.0**

- Version historique


**V3.0 VS V2.0**

![V3 vs V2](https://raw.github.com/hallard/teleinfo/main/uTeleinfo/pictures/MicroTeleinfo-3vs2.png)


# Installation et configuration

Les tests suivants sont réalisés depuis un terminal (ou via SSH) sur Rapsberry PI, veillez à bien toujours avoir votre PI à jour avec les commandes suivantes


```shell
sudo apt-get update
sudo apt-get upgrade
```

## Verification du module

Lancer un terminal (ou via SSH), connecter le module puis faites un `dmesg -e`

```shell
[Dec27 11:25] usb 1-1.4: new full-speed USB device number 30 using dwc_otg
[  +0.144735] usb 1-1.4: New USB device found, idVendor=1a86, idProduct=55d4, bcdDevice= 4.43
[  +0.000022] usb 1-1.4: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[  +0.000016] usb 1-1.4: Product: uTinfo-V3.0
[  +0.000015] usb 1-1.4: Manufacturer: CH2i
[  +0.000014] usb 1-1.4: SerialNumber: TINFO-2000
[  +0.001248] cdc_acm 1-1.4:1.0: ttyACM0: USB ACM device
```

On voit bien que le module est bien detecté, dans notre cas le nom de device `/dev/ttyACM0`

## Connecter la téléinfo

Brancher le connecteur vert du module avec un cable (un petit cable fait parfaitement l'affaire comme téléphonique, réseau, alim LED, ...) sur le compteur aux bornes I1 et I2, peut importe le sens ce n'est pas polarisé.

La Led bleu doit clignoter très rapidement (ou être quasi allumée) indiquant que le signal téléinfo est bien présent.

## Test de réception 

### Picocom

Le 1er test est avec un terminal série nommé `picocom` , installation via 

```shell
sudo apt-get install picocom
```

Puis lancer la commande suivante si vous êtes en mode historique
```shell
root@pi03:~ # picocom -b 1200 -d 7 -p e -f n /dev/ttyACM0 
```

Ou celle ci en mode standard
```shell
root@pi03:~ # picocom -b 9600 -d 7 -p e -f n /dev/ttyACM0 
```

Vous devriez voir arriver des données sur le termimal 

```shell
picocom v3.1

port is        : /dev/ttyACM0
flowcontrol    : none
baudrate is    : 1200
parity is      : even
databits are   : 7
stopbits are   : 1
escape is      : C-a
local echo is  : no
noinit is      : no
noreset is     : no
hangup is      : no
nolock is      : no
send_cmd is    : sz -vv
receive_cmd is : rz -vv -E
imap is        : 
omap is        : 
emap is        : crcrlf,delbs,
logfile is     : none
initstring     : none
exit_after is  : not set
exit is        : no

Type [C-a] [C-h] to see available commands
Terminal ready
HCHP 002784502 /
PTEC HP..  
IINST 001 X
IMAX 002 A
PAPP 00200 #
HHPHC A ,
MOTDETAT 000000 B
ADCO 021528603314 :
OPTARIF HC.. <
ISOUSC 15 <
HCHC 001096011 X
HCHP 002784502 /
PTEC HP..  
IINST 001 X
IMAX 002 A
PAPP 00180 *
HHPHC A ,
MOTDETAT 000000 B
ADCO 021528603314 :
OPTARIF HC.. <

Terminating...
Skipping tty reset...
Thanks for using picocom
root@pi03:~ # 

```

PS : Pour quitter picocom il faut faire `CTRL-A` puis immédiatement `CRTL-Q`

De plus lorsque picocom est actif et les données reçues par le PI la LED rouge doit aussi se mettre à clignoter 

![Schematics](https://raw.github.com/hallard/teleinfo/main/uTeleinfo/pictures/MicroTeleinfo-blink.gif)


### Test avec Python

Un programme de test basic a aussi été réalisé en python, vous trouverez toute la documentation sur le repo suivant
https://github.com/hallard/teleinfo-test

### Python avancé

Un programme plus avancé été réalisé en python (merci à l'auteur original), vous trouverez toute la documentation sur le repo suivant
https://github.com/hallard/python-teleinfo


# Conception

**Schémas** 

![Schematics](https://raw.github.com/hallard/teleinfo/main/uTeleinfo/pictures/MicroTeleinfo-sch.png)

**Cartes**  

![PCB](https://raw.github.com/hallard/teleinfo/main/uTeleinfo/pictures/MicroTeleinfo-pcb.png)

![On PI](https://raw.github.com/hallard/teleinfo/main/uTeleinfo/pictures/MicroTeleinfo-plugged.png)


# Support et discussions

Si vous rencontrez le moindre soucis ou si vous voulez simplement discuter de votre projet avec ce module n'hésitez pas à utiliser le [forum communautaire](https://community.ch2i.eu/category/9)

# License

<img alt="Creative Commons Attribution-NonCommercial 4.0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png">   

This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/)    
If you want to do commercial stuff with this project, please contact [CH2i company](https://ch2i.eu/en#support) so we can organize an simple agreement.

# Trop compliquer de le fabriquer soit même ? 

Vous pouvez commander ce module assemblé et prêt à l'emploi sur [tindie][1]

<a href="https://www.tindie.com/products/25467/"><img src="https://d2ss6ovg47m0r5.cloudfront.net/badges/tindie-mediums.png" alt="I sell on Tindie" width="150" height="78"></a>


[1]: https://www.tindie.com/products/28873/
[4]: http://hallard.me
[5]: http://hallard.me/teleinfo/
[6]: https://www.tindie.com/products/Hallard/micro-teleinfo/
