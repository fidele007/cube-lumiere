#Cube de lumière

Pour le premier projet à SciLab, j’ai construit un cube de lumière en bois à l’aide de découpe laser, et la lumière provenant du LED attaché à l’Arduino UNO. Sur chaque façade du cube, il se trouve un dessin d’un animal dont le cygne, l’araignée, le perroquet et l’aigle. 

##Construction de cube
Les étapes pour construire le cube :

1.	Récupérer un plan de cube à l’aide du site http://boxdesigner.connectionlab.org/ . Les dimensions choisies sont : 90mm x 90mm x 90mm et l’épaisseur de 5mm. Le site vous générera un fichier PDF.
2.	Importer le fichier dans Inkscape ou bien Illustrator et mettre sur chaque façade un dessin de votre choix.
3.	Modifier la dimension de votre document selon votre machine de laser-cut. Dans mon cas, le document doit être de taille 609mm x 304mm.
4.	Imprimer votre produit avec un découpe laser et assembler les pièces !

##Arduino
Un LED et une photocellule sont placés sur l’Arduino. La photocellule fonctionne comme un capteur de lumière en fonction duquel le LED s’allume ou pas. Pour alimenter l’Arduino, on utiliser une batterie de 5V.
Le code simple pour Arduino :

```{cpp}
int potPin = A0;
int led_r = 9;
int led_g = 10;
int led_b = 11;
//int val = 0;
int red = 0;
int green;
int blue = 0;
int fadeAmount = 3;

int random = 0;

void setup() {
  Serial.begin(9600);
  pinMode(potPin, INPUT);
  
  pinMode(led_r, OUTPUT);
  pinMode(led_g, OUTPUT);
  pinMode(led_b, OUTPUT);
}

void loop() {
  analogWrite(led_r, red);
  analogWrite(led_b, blue);
  analogWrite(led_g, green);

  if (red == 0) {
    delay(1000);
  }
  red = red + fadeAmount;
  blue = blue + fadeAmount;
  green = green + fadeAmount;
  
  if (red == 0 || red == 255) {
    fadeAmount = -fadeAmount;
    int i = random(0, 5);
  }

  delay(30);
}
```

(À remplir)

Regardons les résultats lorsqu’il fait nuit !
