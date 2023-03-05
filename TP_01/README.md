
# Laboratoire 0 : Applications d'introduction

Basee sur https://ocw.cs.pub.ro/courses/pm/lab/lab0-2022

-----------------------------------------

 [**la fiche technique ATmega328P**](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf "https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf")


## 1. Introduction

Vous venez de construire un nouveau PC et de mettre RVB sur la RAM, sur les ventilateurs, sur le boîtier, beaucoup. Vient maintenant la question, qui « appuie » sur les boutons pour que les couleurs sortent au fur et à mesure que vous les mettez dans le logiciel ? Le CPU doit-il rester et s'occuper de changer les couleurs ? À mon avis, c'est un peu un gaspillage de ressources. C'est là qu'un microcontrôleur entre en jeu ! Il s'agit d'un processeur plus petit et plus simple, avec une tâche bien définie dans le système. Son travail dans notre cas est de contrôler le RVB toute sa vie.

### 1.2. Qu'est-ce qu'un microcontrôleur (µC) ?

Un ordinateur sur puce. Plus en détail, il s'agit d'un circuit intégré qui regroupe une unité de traitement (CPU), des mémoires (RAM volatile, EEPROM non volatile, Flash, ROM) et divers périphériques lui permettant de communiquer avec l'environnement extérieur.

### 1.3. µC dans la nature

On les retrouve dans différents appareils tels que : les téléphones, les appareils électroménagers, les satellites, les avions, dans les usines, etc. Il existe une large gamme de microcontrôleurs disponibles et ils sont choisis en fonction de l'application, en tenant compte notamment de l'optimisation du coût et de la consommation d'énergie pour l'appareil où le µC doit être utilisé.

### 1.4. Que trouve-t-on dans un µC ?

-   **L'unité centrale de traitement** (cœur µP) avec une architecture pouvant être de 8, 16, 32 ou 64 bits
    
-   Mémoire de données volatile ( **RAM** ) et/ou non volatile (Flash ou EEPROM)
    
-   Mémoire programme non volatile ( **Flash** ou **EEPROM** )
    
-   Ports d'entrée-sortie numériques à usage général ( **GPIO** - Sortie d'entrée à usage général)
    
-   Interfaces de communication série ( **USART** , **SPI** , **I2C** , PCM, **USB** , SDIO, etc.)
    
-   Interfețe ethernet
    
-   Interfețe pentru afișaj grafic (LVDS,  **HDMI**  sau alte protocoale dedicate controlului afișajelor LCD)
    
-   Timere (cu rol intern, sau utilizate în generarea semnalelor periodice -ex: PWM-, sau ca watchdog)
    
-   Convertoare analog-digitale și digital-analogice (**ADC**,  **DAC**), front-end-uri analogice și alte circuite dedicate semnalelor analogice
    
-   Sursă de tensiune integrată
    
-   Interfață pentru programare şi debugging
    

  

**Les périphériques** sont tout appareil, interne ou externe, qui se connecte à un système informatique et étend ses fonctionnalités de base. Dans le cas du microcontrôleur, il existe un certain nombre de tels périphériques inclus directement dans le circuit intégré (exemples ci-dessus). Bien qu'ils ne ressemblent pas aux périphériques d'un PC (écran, carte graphique, clavier, souris, etc.), sans eux le microcontrôleur ne pourrait pas interagir avec l'environnement extérieur. De plus, les périphériques nous aident à connecter d'autres éléments plus puissants au contrôleur et à pouvoir offrir des fonctionnalités similaires à un système PC (connexion Internet, ligne de données USB, affichage graphique, etc.)

## 2.  µC AVR (Atmel)

Pendant le semestre, nous travaillerons avec des microcontrôleurs de la famille AVR de Microchip. Ils ont une architecture Harvard 8 bits et un jeu d'instructions réduit (RISC).


### 2.1. ATmega328P

![atmega324|  Un ATmega324 µC en capsule PDIP](https://ocw.cs.pub.ro/courses/_media/pm/lab/lab0/atmel-atmega324p-20pu.jpg?w=250&tok=f939f8 "atmega324|  Un ATmega324 µC en capsule PDIP")

Nous allons travailler avec ce petit. Il s'agit d'un microcontrôleur 8 bits de la famille megaAVR. Les registres et le bus de données interne sont de 8 bits.

**Les spécifications du µC**

-   32 KB Flash (determină dimensiunea maximă a programului care poate fi executat)
    
-   1 KB EEPROM
    
-   2 KB RAM
    
-   20  MHz  frequence maximale 
    
-   Alimentation a  1.8V  et 5.5 V
    
-   6 Canal de PWM
    
-   8 canal de ADC,  avec resolution de 10 biți
    
-   3 ports digitale de I/O (GPIO - General Purpose I/O) , avec 7 ou 8 pins (23 en total)
    
-   3 timers (2 sur 8 biți et 1 sur 16 biți)
    
-   Interface de communication seriale : USART, SPI, TWI
    
-   Interfaţe de programmation ISP et debug JTAG
    

  

Avec ce microcontrôleur, nous apprendrons à configurer ses broches et à interagir avec l'environnement externe à partir du code. Il comporte 28 broches (illustrées ci-dessous), dont 5 pour l'alimentation ou les fonctions auxiliaires et 23 pour les E/S. le microcontrôleur a quatre ports : A, B, C et D ; dont seuls les 3 derniers nous sont accessibles via des broches externes.

![Configuration des broches pour ATmega328P](https://ocw.cs.pub.ro/courses/_media/pm/lab/ic-atmega328-pu-3.jpg?w=400&tok=d37bc1 "Configuration des broches pour ATmega328P")

Pour plus de détails, vous pouvez toujours consulter [la fiche technique](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf "https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf") (documentation technique résumée) du microcontrôleur. :)

### 2.2. Plaque de laboratoire

La Fondation Arduino a créé des ensembles de cartes de développement open source, cartes que nous utiliserons également en laboratoire. Une carte de développement est un circuit qui fournit facilement les broches µC dessus et qui contient l'alimentation, la protection et éventuellement le programmateur µC.

Le programmateur est une puce spéciale, parfois même un microcontrôleur, qui a pour rôle de charger le code dans la mémoire µC. Nous ne pouvons pas connecter le µC directement aux fils USB d'un PC, nous avons besoin de quelqu'un pour "traduire" les informations respectives.

Brochage Arduino UNO : ![Pionnier Arduino UNO](https://ocw.cs.pub.ro/courses/_media/pm/lab/arduino-uno-pinout-diagram.png?w=600&tok=b3c0e1 "Pionnier Arduino UNO")

### 2.3. Framework-ul Arduino

Bien qu'en laboratoire, nous vous ferons travailler directement avec des adresses mémoire pour configurer les broches et les contrôler, nous vous montrerons également comment utiliser un framework appelé Arduino. Il est écrit en C++ et est essentiellement une bibliothèque avec des fonctions d'assistance et des en-têtes (fichiers .h) avec des définitions pour chaque processeur individuel. Comme tout niveau d'abstraction supplémentaire, il facilite le développement, mais apporte également une pénalité de performance parfois même 20 fois plus lente. 

## 3. Mettons-nous au travail

### 3.1. Actionneurs et Transducteurs (Capteurs)

Pour pouvoir s'interfacer avec l'environnement extérieur, on utilise différents composants électroniques qui ont le rôle soit d'actionneur (modifie l'état de l'environnement extérieur) soit de transducteur/capteur (ils sont influencés par l'environnement extérieur et fournissent des informations au microcontrôleur sur divers paramètres).

**Exemples d'actionneurs** :

-   Ventilateurs;
    
-   Indicateurs sonores - buzzers ;
    
-   Indicateurs lumineux ;
    
-   Résistances chauffantes.
    

Parfois, pour activer un actionneur, un actionneur est nécessaire. Par exemple, si on veut démarrer un moteur, le µC donne juste un ordre de démarrage logique à un transistor qui va s'ouvrir et laisser passer un courant important (ici par "courant fort" on compare au maximum quelques milliampères qu'un µC peut sortir).

**Exemples de capteurs** :

-   Boutons;
    
-   Photorésistances - sa résistance électrique est influencée par la quantité de lumière ;
    
-   Thermistances - leur résistance électrique est influencée par la température.
    

Selon le type de transducteurs, ils peuvent nécessiter un traitement du signal avant qu'il ne soit pris en charge par µC (conditionnement du signal) - par exemple la photorésistance doit être utilisée dans un montage avec un diviseur de tension ou une source de courant -, ou ils peuvent être connectés directement au microcontrôleur - par exemple le bouton.

#### 3.1.1. LED

Les LED - Light Emitting Diode - également appelées diodes électroluminescentes émettent de la lumière lorsqu'elles sont directement polarisées. Ne confondez pas les ampoules car elles ont des modes de fonctionnement radicalement différents.


Les LED peuvent être utilisées comme voyants lumineux (souvent utilisés dans divers appareils pour signaler que l'appareil est allumé et fait quelque chose), ou pour l'éclairage, auquel cas des LED d'alimentation sont utilisées. En laboratoire, des LED sont utilisées pour indiquer l'état d'une broche.

##### Calcul de la résistance de limitation de courant

Les LED sont des diodes, de sorte que le courant qui les traverse augmente de façon exponentielle à mesure que la tension appliquée augmente. Pour utiliser une LED pour indiquer l'état d'une broche (plutôt dit pour indiquer la présence de tension), le courant traversant la LED doit être limité. Cela peut être fait de la manière la plus simple en enchaînant une résistance avec la LED.

Une LED est conçue pour fonctionner à un courant nominal (ex : 10mA). La chute de tension sur les LEDs indicatrices, de faible puissance, à ce courant est donnée par la chimie de la LED, qui donne aussi sa couleur. En laboratoire, puisque nous utilisons une LED avec un si petit courant, nous pouvons l'alimenter directement à partir des broches logiques du µC.

Le schéma utilisé est le suivant :

![Schéma de fonctionnement et calcul de la résistance de limitation de courant à travers la LED](https://ocw.cs.pub.ro/courses/_media/pm/lab/lab0/r_limit.png?w=400&tok=6de8ab "Schéma de fonctionnement et calcul de la résistance de limitation de courant à travers la LED")

**Solution** : Si l'alimentation du microcontrôleur est de 5V pour une LED rouge que l'on souhaite utiliser à 10mA, spécifié par le constructeur avec une chute de tension de 1,7V, il faut utiliser une résistance de 330 ohms.

#### 3.1.2. Boutons

Le moyen le plus simple pour l'utilisateur d'interagir avec un microcontrôleur consiste à utiliser des boutons. La manière de connecter un bouton poussoir dans ce laboratoire est donnée dans la figure ci-dessous :

![Raccordement d'un bouton-poussoir : a) incorrect, avec entrée flottante, b) correct, avec résistance de pull-up](https://ocw.cs.pub.ro/courses/_media/pm/lab/lab0/conectare_buton.png?w=400&tok=10473c "Raccordement d'un bouton-poussoir : a) incorrect, avec entrée flottante, b) correct, avec résistance de pull-up")

Raccordement d'un bouton-poussoir :
 a) incorrect, avec entrée flottante, b) correct, avec résistance de pull-up

**a)** Affiche un bouton connecté à la broche PD0 de µC. Lorsque le bouton est enfoncé, l'entrée PD0 sera connectée à GND, elle sera donc à l'état logique "0". Cette méthode de liaison est incorrecte car lorsque le bouton n'est pas enfoncé, l'entrée est **dans un état indéfini** (comme si elle était laissée en l'air), n'étant connectée ni à GND ni à Vcc ! Cet état est appelé **état d'impédance augmentée** . En pratique, si nous lisons maintenant la valeur de la broche, elle produira un résultat de 1 ou 0 selon les conditions environnementales. Par exemple, si nous rapprochons notre doigt de cette entrée, la lecture sera 1, et si nous écartons notre doigt, la lecture sera 0.

**b)** Montrez la manière correcte de connecter le bouton, en utilisant une **résistance de rappel** entre la broche d'entrée et Vcc. Cette résistance a pour rôle de ramener l'entrée à l'état logique "1" lorsque le bouton est libre en "remontant" le potentiel de la ligne à Vcc. **Alternativement, une résistance pull-down** (connectée à GND) peut être utilisée , auquel cas l'entrée est maintenue à l'état logique "0" tant que le bouton n'est pas enfoncé.

**Au labo** : Pour économiser de l'espace extérieur, dans l'ATmega328P µC, ces résistances ont été incluses à l'intérieur du circuit intégré. Au départ, ils sont désactivés et leur activation peut se faire via un logiciel.

### 3.2. Travailler avec des registres

Cela peut être considéré comme l'étape la plus importante lorsque nous voulons utiliser un µC. Nous devons apprendre à configurer en interne le µC pour exécuter les fonctions que nous voulons. Dans ce laboratoire, nous allons configurer les broches pour qu'elles soient des E/S : certaines broches pour lire si elles sont sous tension (entrée) ou d'autres pour donner 0 volt ou 5 volts en fonction de nos commandes logicielles (sortie).

Pour clarifier une terminologie : maintenant **, lorsque nous disons que nous définissons un registre** ou écrivons dans un registre, nous ne faisons pas référence aux registres à usage général du processeur avec lequel vous avez travaillé à l'IOCLA. **On parle d'adresses mémoire** réservées en µC. Nous répéterons encore ensemble : « Par registre nous entendons une adresse mémoire ». En gros, comment ça se passe "sous le capot", ces octets ont des connexions physiques et si on écrit un bit 1 ou 0 à certaines positions, on active ou désactive des éléments µC.

Toujours pour savoir comment configurer un périphérique, [la](https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf "https://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf") fiche technique µC est le meilleur endroit pour savoir exactement où se trouve chaque configuration et ce qu'elle fait. Comme il n'est pas du tout agréable d'écrire en code `*(0x04) = 0b0000 0001`, les constructeurs de µC proposent des bibliothèques ( [avr-libc](http://www.nongnu.org/avr-libc/user-manual/modules.html "http://www.nongnu.org/avr-libc/user-manual/modules.html") ) pour donner des noms à ces adresses. La feuille de triche suivante vous montrera comment configurer un certain bit à partir d'une certaine adresse à l'aide de macros (qui sont également considérées comme les meilleures pratiques).

Pour écrire une valeur dans un registre, on peut affecter directement une valeur, mais cela ne rend pas le code très facile à comprendre. _Notre recommandation_ est d'utiliser des masques de bits ( `(1 << x)`) ou la macro `_BV(x)`. Nous recommandons également d'utiliser le nom de la broche au lieu de son index, car leurs noms suggèrent la fonction qu'ils exécutent (par exemple, dans le `ADCSRA`registre du convertisseur analogique-numérique, le bit `ADEN`est le bit d'activation). Les noms de broches, comme pour les registres, sont déjà définis dans avr-libc.

Opération

Ecrire bit sur 1

    macro |=  ( 1  << index_bit ) 

Écrire le bit à 0

    macro &= ~ ( 1  << index_bit ) 

Basculer le bit

    macro ^=  ( 1  << index_bit ) 

Lire un bit

    macro &  ( 1  << index_bit ) 


Vous pouvez également lire les détails sur l'utilisation des bits de registre dans la section des didacticiels sur le [wiki](https://ocw.cs.pub.ro/courses/pm/tutorial/biti "https://ocw.cs.pub.ro/courses/pm/tutorial/biti") .


### 3.3. Registres d'E/S

Pour ce labo, je vous ai évité de chercher dans la fiche technique, ce qui peut sembler compliqué au premier abord, et je vous ai résumé quelles adresses mémoire (registres) vous devez modifier pour travailler dans ce labo.

Le microcontrôleur ATmega324 offre 4 ports d'E/S de 8 broches chacun, et en interne, chaque port est associé à trois registres 8 bits à travers lesquels l'utilisateur peut contrôler le flux de données au niveau des broches : il peut écrire/lire des données vers/ _depuis_ le port respectif. Ces trois registres sont :

-   **`DDRn`**- _Registre de direction des données_
    
    -   définit la direction des broches du port
        
    -   si le bit _x_ a la valeur **0** alors la broche _x_ est **entrée**
        
    -   si le bit _x_ est **1** alors la broche _x_ est **sortie**
        
-   **`PORTn`**- _Registre de données_
    
    -   définir les valeurs de sortie des broches ou activer/désactiver les résistances pull-up
        
    -   si le bit _x_ a la valeur **0** alors
        
        -   si la broche _x_ est **sortie** , elle aura la valeur **LOW**
            
        -   si la broche _x_ est **entrée** , la résistance pull-up sera **désactivée**
            
    -   le bit de données _x_ a la valeur **1** alors
        
        -   si la broche _x_ est **sortie** , elle aura la valeur **HIGH**
            
        -   si la broche _x_ est **entrée** , la résistance pull-up sera **activée**
            
-   **`PINn`**- _Adresse des broches d'entrée_
    
    -   si la broche est **entrée** , nous pouvons lire les données du port respectif
        
        -   si la broche _x_ a la valeur **LOW** alors le bit _x_ aura la valeur **0**
            
        -   si la broche _x_ a la valeur **HIGH** alors le bit _x_ aura la valeur **1**
            
    -   si la broche est **sortie** , nous pouvons basculer automatiquement
        
        -   si nous écrivons la valeur **1 au bit** _x_ , le bit équivalent dans PORTn sera automatiquement annulé (c'est-à-dire que nous **basculerons** automatiquement l'état de la broche)
            

_n_ peut être A, B, C ou D selon le port sélectionné. _x_ peut être compris entre 0 et 7.

La description détaillée des ports et de leurs registres correspondants se trouve dans la fiche technique ATmega324, dans le chapitre _Ports d'E/S_ .

#### 3.3.1. Exemple de travail avec des sorties

Disons que nous avons une LED liée à la broche 1 du port B (appelée `PORTB1`ou `PB1`). Pour allumer ou éteindre la LED, nous devons suivre les étapes suivantes :

-   La broche `PB1`doit être configurée comme une sortie
    
    -   Le bit 1 ( `PB1`) dans le registre `DDRB`sera 1
        
    -   `DDRB |= (1 « PB1);`
        
-   Pour allumer la LED, la broche doit `PB1`être HAUTE
    
    -   Le bit 1 ( `PB1`) dans le registre `PORTB`sera 1
        
    -   `PORTB |= (1 « PB1);`
        
-   Pour éteindre la LED, la broche doit être `PB1`réglée sur LOW
    
    -   Le bit 1 ( `PB1`) dans le registre `PORTB`sera 0
        
    -   `PORTB &= ~(1 « PB1);`
        

#### 3.3.2. Exemples de travail avec des entrées

Supposons que nous ayons un bouton connecté à la broche 4 du port D (appelé `PORTD4`ou `PD4`), du cas b) montré précédemment. Pour déterminer l'état du bouton (enfoncé ou libre), nous devons suivre les étapes suivantes :

-   La broche `PD4`doit être configurée comme une entrée
    
    -   Le bit 4 ( `PD4`) dans le registre `DDRD`sera 0
        
    -   `DDRD &= ~(1 « PD4);`
        
-   Pour déterminer l'état de poussée du bouton, nous devons lire la valeur de la broche à laquelle il est attaché. Ce sera 1 lorsque le bouton est libre et 0 lorsque le bouton est enfoncé
    
    -   Nous lisons la valeur du bit 4 ( `PD4`) du registre`PIND`
        
    -   `char val = PIND & (1 « PD4);`
        

  

##### Hello World avec registres

Sur la carte Arduino UNO, il y a une LED intégrée connectée à la broche PB5 du µC. Nous allons écrire un programme qui allume et éteint cette LED à des intervalles de 500 ms, cela se fait en changeant la tension sur la broche.


    // il est preferable d'utiliser des macro pour ne pas hardcodee
    // si on veut changer la broche on a une seule ligne a modifier
    #define LED PB5
     
    void setup() {
      DDRB |= (1 << LED);
      // sau
      // DDRB |= _BV(LED);
    }
     
    void loop() {
      PORTB |= (1 << LED); // ori _BV(LED)
      delay(500); // attends 500 ms
      PORTB &= ~(1 << LED); // ori _BV(LED)
      delay(500);
    }

### 3.4. Framework-ul Arduino

Ce cadre apporte un niveau d'abstraction supplémentaire qui masque le travail avec les registres et les opérations sur les bits derrière certaines fonctions plus lentes mais pratiques.

#### 3.4.1 Configuration de l'IDE

-   L'outil le plus courant proposé par la fondation Arduino est [l'IDE Arduino](https://www.arduino.cc/en/software "https://www.arduino.cc/en/software") . Il offre la prise en charge de plusieurs cartes et processeurs AVR, ainsi que la possibilité d'installer des bibliothèques tierces, mais à part quelques mises en évidence de la syntaxe, cela ressemble à de la programmation dans Notepad ++.
    

-   Un autre outil utilisé par Microchip est AVR Studio (dépravé) et le plus récent Atmel Studio. C'est un outil professionnel et assez difficile à utiliser à pleine capacité. Si vous voulez l'utiliser, ce serait comme utiliser un flex pour couper votre steak.
    

-   Un troisième IDE possible serait Visual Studio Code avec l'extension de [PlatformIO](https://marketplace.visualstudio.com/items?itemName=platformio.platformio-ide "https://marketplace.visualstudio.com/items?itemName=platformio.platformio-ide") pour avoir un accès automatique aux bibliothèques µC et y charger le code, tout en ayant intellisense.
    

Parlez-en au laborantin pour lequel IDE il propose/utilise afin qu'il puisse vous aider plus facilement si vous rencontrez des difficultés.

#### 3.4.2. Le premier code

    #include "µC_header" // on importe cette biblioteque pour avoir de macro utiles
     
    void setup() { // le setup initiale pour la configuration du  µC-ului
    }
     
    void loop() { //le code qui roule en boucle infinie equivalent a while(1){}
    }

______________________________________________________

 

#### 3.4.3. Travailler avec les E/S à l'aide d'Arduino

Ce sont les 3 fonctions de base fournies par le framework Arduino pour travailler avec des broches comme E/S.

Désormais, lorsque nous voulons faire référence à une broche spécifique, nous utiliserons l'identifiant de la broche tel qu'il apparaît sur la carte Arduino utilisée (de 0 à 13 ou de A0 à A7).

[PinMode()](https://www.arduino.cc/reference/en/language/functions/digital-io/pinmode/ "https://www.arduino.cc/reference/en/language/functions/digital-io/pinmode/")

Est utilise pour configurer si la broche (pin) est de type input (entree) ou output (sortie) avec pull-up


Signature:  `void pinMode(pin, mode)`

pin: du 0 a 13 ou A0 a A7

mode: INPUT, OUTPUT, ou INPUT_PULLUP

[digitalWrite()](https://www.arduino.cc/reference/en/language/functions/digital-io/digitalwrite/ "https://www.arduino.cc/reference/en/language/functions/digital-io/digitalwrite/")

Est utiliser pour seter un pin sur `HIGH` (5V) ou `LOW` (0V)


Signature:  `void digitalWrite(pin, value)`


[digitalRead()](https://www.arduino.cc/reference/en/language/functions/digital-io/digitalread/ "https://www.arduino.cc/reference/en/language/functions/digital-io/digitalread/")

Retourne `HIGH` (1) ou `LOW` (0) en fonction de la valeur en V entree sur la broche.

Signature:  `int digitalRead(pin)`

  

##### Hello World avec Arduino

La LED incorporée sur la carte Aduino UNO est connectée à la broche 13 dans le schéma de marquage du framework Arduino.

    #define LED 13 // il est préférable de toujours utiliser des macros, pas des valeurs hardcodee
     
    void setup ( )  { pinMode ( LED , OUTPUT ) ; 
    }
     
    void loop ( )  { digitalWrite ( LED , HIGH ) ; délai ( 500 ) ; digitalWrite ( LED , FAIBLE ) ; délai ( 500 ) ; 
    }

#### 3.4.4. Comparaison des fonctions Arduino et travail avec les registres

![](https://ocw.cs.pub.ro/courses/_media/pm/lab/untitled.png)(https://ocw.cs.pub.ro/courses/_media/pm/lab/untitled.png)

## 4. Simulateur Tinkercad

Tinkercad est un programme de modélisation 3D gratuit en ligne reconnu pour sa facilité d'utilisation. Il fournit un support de simulation pour la carte Arduino UNO avec certaines limitations.

[https://www.tinkercad.com](https://www.tinkercad.com/ "https://www.tinkercad.com")

## 5. Exercices

Circuit de travail

![Circuit de travail](https://ocw.cs.pub.ro/courses/_media/pm/lab/lab0-2021.png?w=500&tok=8eadba)

-----------------------------------------------------------------------------


**Tâche 0** Nous commençons par vérifier la configuration. Installez l'IDE souhaité/utilisé et exécutez l' exemple **Hello Word** avec des registres sur la carte Arduino UNO dans le laboratoire. Ensuite, testez la version dans laquelle vous utilisez le framework Arduino pour voir que le même comportement est proposé.

**Tâche 1** Vérifiez que le circuit sur les planches à pain du laboratoire remplit la même fonction que le circuit de l'image ci-dessus et que l'Arduino est connecté de la même manière.

**Tâche 2** À l'aide du bouton connecté à la broche numérique 2 (PD2), configurez les registres pour que la broche d'entrée s'allume et que la LED s'allume lorsque vous appuyez sur le bouton. (n'oubliez pas d'activer la résistance pull-up interne du µC)

**Tâche 3** Maintenant, à l'aide du bouton connecté à la broche numérique 3 (vous pouvez trouver la macro de la broche), configurez-le également pour utiliser les boutons pour augmenter et diminuer la durée du délai utilisé dans l'exemple Hello **Word** .

Après avoir terminé les exercices et si vous avez encore du temps/de la puissance, nous vous recommandons de résoudre les exercices 2 et 3 en utilisant les fonctions du framework Arduino.

## 6. Lien-uri utile

-   [Les monstres de l'AVR](http://www.avrfreaks.net/ "http://www.avrfreaks.net/")
    
-   [Bibliothèque AVR](http://www.nongnu.org/avr-libc/user-manual/modules.html "http://www.nongnu.org/avr-libc/user-manual/modules.html")
    
-   [CCG AVR](https://gcc.gnu.org/wiki/avr-gcc "https://gcc.gnu.org/wiki/avr-gcc")
    
-   [Détails d'introduction sur la programmation AVR (blog)](http://hackaday.com/2010/10/23/avr-programming-introduction/ "http://hackaday.com/2010/10/23/avr-programming-introduction/")
    
-   [Comment fonctionnent les LED](http://electronics.howstuffworks.com/led.htm "http://electronics.howstuffworks.com/led.htm")

