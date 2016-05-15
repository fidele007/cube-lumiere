#Cube de lumière

Pour le premier projet à SciLab, j’ai construit un cube de lumière en bois à l’aide de découpe laser, et la lumière provenant du LED attaché à l’Arduino UNO. Sur chaque façade du cube, il se trouve un dessin d’un animal dont le cygne, l’araignée, le perroquet et l’aigle. 

##Construction de cube
Les étapes pour construire le cube :

1.	Récupérer un plan de cube à l’aide du site http://boxdesigner.connectionlab.org/. Les dimensions dont j'ai choisies sont : 90mm x 90mm x 90mm et l’épaisseur de 5mm. Le site vous générera un fichier PDF.
2.	Importer le fichier dans **Inkscape** ou bien **Illustrator** et mettre sur chaque façade un dessin de votre choix.
3.	Modifier la dimension de votre document selon votre machine de laser-cut. Dans mon cas, le document doit être de taille 609mm x 304mm.
4.	Imprimer votre produit avec un découpe laser et assembler les pièces !

![Cube et Arduino](https://github.com/fidele007/cube-lumiere/raw/master/cube_arduino.JPG)

##Arduino
Un LED et une photocellule sont placés sur l’Arduino. La photocellule fonctionne comme un capteur de lumière en fonction duquel le LED s’allume ou pas. Pour alimenter l’Arduino, on utilise une batterie de 5V.

La couleur de LED change d'une couleur à une autre de manière aléatoire parmi les différentes combinaisons entre les trois couleurs principales dont le rouge, le vert et le bleu.

Le code pour Arduino :

```{cpp}
// Pin pour la photocellule
int potPin = A0;

int light = 0;

// Les trois pins de couleur RGB
int led_r = 9;
int led_g = 10;
int led_b = 11;

int red = 0;
int green;
int blue = 0;

int fadeAmountRed = 5;
int fadeAmountGreen = 5;
int fadeAmountBlue = 5;

// Variable aléatoire
long randNumber;

boolean shouldTurnOn = false;

void setup() {
  Serial.begin(9600);
  pinMode(potPin, INPUT);
  
  pinMode(led_r, OUTPUT);
  pinMode(led_g, OUTPUT);
  pinMode(led_b, OUTPUT);
}

void loop() {
  light = analogRead(potPin);
  Serial.println(light);

  if (red == 0 || green == 0 || blue == 0) {
    if (light < 200) {
      shouldTurnOn = true;
    } 
    else {
      shouldTurnOn = false;
    }
  }

  // Le LED s'allume lorsqu'il aperçoit qu'il fait nuit
  if (shouldTurnOn) {
    // Choisir une nouvelle couleur pour le LED
    if (randNumber == 0) {
      analogWrite(led_r, red);
    }
    else if (randNumber == 1) {
      analogWrite(led_g, green);
    }
    else if (randNumber == 2) {
      analogWrite(led_b, blue);
    }
    else if (randNumber == 3) {
      analogWrite(led_r, red);
      analogWrite(led_g, green);
    }
    else if (randNumber == 4) {
      analogWrite(led_r, red);
      analogWrite(led_b, blue);
    }
    else if (randNumber == 5) {
      analogWrite(led_r, green);
      analogWrite(led_b, blue);
    }
  
    // Attente de 1s lorsque le LED s'éteint
    if (red == 0 || green == 0 || blue == 0) {
      digitalWrite(led_r, LOW);
      digitalWrite(led_g, LOW);
      digitalWrite(led_b, LOW);
      delay(1000);
    }
    
    red = red + fadeAmountRed;
    green = green + fadeAmountGreen;
    blue = blue + fadeAmountBlue;
  
    // Incrémenter ou décrementer la puissance de LED
    if (red == 0 || red == 255) {
      fadeAmountRed = -fadeAmountRed;
    }
  
    if (green == 0 || green == 255) {
      fadeAmountGreen = -fadeAmountGreen;
    }
  
    if (blue == 0 || blue == 255) {
      fadeAmountBlue = -fadeAmountBlue;
    }
  
    // Générer un nombre random entre 0 et 5 pour choisir une nouvelle couleur
    if (red == 0 || green == 0 || blue == 0) {
       randNumber = random(5);
    }
  
    // Attente de 40 millisecondes pour mieux voir la transition
    delay(40);
  }
  
  // S'il fait jour, le LED s'atteinte automatiquement
  // et remettre toutes les valeurs à zero
  else {
    digitalWrite(led_r, LOW);
    digitalWrite(led_g, LOW);
    digitalWrite(led_b, LOW);
    red = 0;
    green = 0;
    blue = 0;
  }
}
```

Regardons les résultats lorsqu’il fait nuit !

![Aigle](https://github.com/fidele007/cube-lumiere/raw/master/aigle.JPG)
![Araignee](https://github.com/fidele007/cube-lumiere/raw/master/araignee.JPG)
![Cygne](https://github.com/fidele007/cube-lumiere/blob/master/cygne.JPG)
![Perroquet](https://github.com/fidele007/cube-lumiere/raw/master/perroquet.JPG)

Lien vers la vidéo de la démonstration : https://vimeo.com/166736057 