Auteur: Magdi Laoun

Cette librairie permet de piloter un driver Trinamic de type TMC5160 à partir d'instructions données selon le protocole SPI.
Elle permet aussi de lire certaines données provenant du driver, toujours à l'aide du protocole SPI.
Le jeu d'instruction est décrit dans la datasheet suivante:
https://www.analog.com/media/en/technical-documentation/data-sheets/TMC5160A_datasheet_rev1.18.pdf
Le protocole SPI utilise 4 fils de connexion entre le microcontrôleur et le driver.
- CLK: clock
- CS: Channel select
- MOSI: Master out, slave in, instruction du microcontrôleur vers le driver
- MISO: Master in, slave out, lecture par le microcontrôleur depuis le driver
En option - EN: Enable, active ou désactive le driver
La fonction begin(...) permet d'associer ces 5 entrées-sorties au microcontrôleur
La fonction init(...) permet de:
  - Régler le courant de repos
  - Régler le courant de marche
  - Régler le nombre de micro-pas par pas (1, 2, 4, 8, 16....256)
Ces 2 fonctions sont à exécuter 1 fois au début du programme.
Les fonctions suivantes sont les plus importantes:
  - setSpeed: permet de fixer la vitesse. Le paramètre est un entier positif et correspond au nombre de pas * nombre de micropas par secondes
  - setAcceleration: permet de fixer l'accélération et la décélération
  - targetPosition: permet de fixer la position d'arrivée du moteur (nombre de pas * nombre de micropas), et valable seulement en mode position
  - setRampMode: permet de définir le type de mouvement selon le paramètre en entrée:
    - 0: mode position
    - 1: mode vitesse CW
    - 2: mode vitesse CCW
  - getSPIPosition: Permet d'obtenir la position actuelle du moteur
Exemple 1: effectuer une rotation de 3 tour
 init(0.05, 0.4, 4); // courant de repos 50 mA, courant de fonctionnement 400 mA, nombre de micropas: 4
 setRampMode(0);
 setAcceleration(100);
 setSpeed(1000);
 targetPosition(2400); // 3 x 200 x 4
Exemple 2: effectuer un mouvement d'accélération continue de valeur 200 dans le sens CW
 setRampMode(1); //mode vitesse
 setSpeed(1000000); //vitesse max très grande pour que l'accélération ne s'interrompt pas
 setAcceleration(200);
Exemple 3: Si l'on souhaite freiner avec une décélération de 200, (c'est-à-dire, accélérer en sens opposé)
 il suffit de renverser le mode vitesse: 
 setRampMode(2);

!!! Attention aux unités !!!
soit pm, vm et am, la position, vitesse, et accélération du moteur en unité du driver. Ce sont des valeurs entières
Soit pr, vr et ar, la position, vitesse et accélération du moteur en unité SI, c'est-à-dire rad, rad/s, rad/s2
Soit n et m, le nombre de pas/tour du moteur (généralement n = 200) et le nombre de micropas par pas (par exemple m=4)
pm = pr/2pi*n*m
vm = 1.37*vr/2pi*n*m
am = 0.015*ar/2pi*n*m
Les coefficients 1.37 et 0.015 ont été déterminés par mesure

This library allows you to control a Trinamic TMC5160 driver using instructions given via the SPI protocol.
It also allows you to read certain data from the driver, again using the SPI protocol.
The instruction set is described in the following datasheet:
https://www.analog.com/media/en/technical-documentation/data-sheets/TMC5160A_datasheet_rev1.18.pdf
The SPI protocol uses four connection wires between the microcontroller and the driver.
- CLK: clock
- CS: Channel select
- MOSI: Master out, slave in, instruction from the microcontroller to the driver
- MISO: Master in, slave out, read by the microcontroller from the driver
Optional - EN: Enable, activates or deactivates the driver
The begin(...) function allows these 5 inputs/outputs to be associated with the microcontroller
The init(...) function allows you to:
  - Set the standby current
  - Set the operating current
  - Set the number of microsteps per step (1, 2, 4, 8, 16....256)
These two functions must be executed once at the start of the programme.
The following functions are the most important:
  - setSpeed: sets the speed. The parameter is a positive integer and corresponds to the number of steps * number of microsteps per second
  - setAcceleration: sets the acceleration and deceleration
  - targetPosition: sets the motor's target position (number of steps * number of microsteps), and is only valid in position mode
  - setRampMode: allows you to define the type of movement according to the input parameter:
    - 0: position mode
    - 1: CW speed mode
    - 2: CCW speed mode
  - getSPIPosition: allows you to obtain the current position of the motor
Example 1: perform a 3-turn rotation
 init(0.05, 0.4, 4); // quiescent current 50 mA, operating current 400 mA, number of microsteps: 4
 setRampMode(0);
 setAcceleration(100);
 setSpeed(1000);
 targetPosition(2400); // 3 x 200 x 4
Example 2: Perform a continuous acceleration movement with a value of 200 in the CW direction
 setRampMode(1); //speed mode
 setSpeed(1000000); //very high maximum speed so that acceleration is not interrupted
 setAcceleration(200);
Example 3: If you want to brake with a deceleration of 200 (i.e. accelerate in the opposite direction)
 simply reverse the speed mode: 
 setRampMode(2);

!!! Pay attention to the units !!!
Let pm, vm and am be the position, speed and acceleration of the motor in driver units. These are integer values.
Let pr, vr and ar be the position, speed and acceleration of the motor in SI units, i.e. rad, rad/s, rad/s2.
Let n and m be the number of steps/revolution of the motor (usually n = 200) and the number of microsteps per step (e.g. m=4).
pm = pr/2pi*n*m
vm = 1.37*vr/2pi*n*m
am = 0.015*ar/2pi*n*m
The coefficients 1.37 and 0.015 were determined by measurement.
