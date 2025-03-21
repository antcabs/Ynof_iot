# Projet Détecteur Environnemental ESP32

## Description
Ce projet implémente un système de détection environnementale utilisant une carte ESP32. Le système est capable de détecter les niveaux de gaz et d'humidité, et active automatiquement un ventilateur lorsque des niveaux anormaux sont détectés. L'état du système est affiché sur un écran LCD 16x2 et des indicateurs visuels sont fournis par des LEDs RGB.

Le projet est basé sur le kit [Keyestudio Smart Home](https://docs.keyestudio.com/projects/KS5009/en/latest/docs/index.html) mais a été adapté pour fonctionner avec une carte ESP32 et MicroPython.

## Composants matériels
- ESP32 DevKit
- Écran LCD 16x2 avec adaptateur I2C
- Capteur de gaz MQ (connecté à ADC)
- Capteur d'humidité/vapeur
- Module de ventilateur
- LED RGB adressables WS2812/NeoPixel
- Bouton poussoir

## Brochage
- **LCD I2C** : SDA → GPIO21, SCL → GPIO22
- **Capteur de gaz** : Signal → GPIO32
- **Capteur d'humidité/vapeur** : Signal → GPIO34
- **Ventilateur** : Contrôle → GPIO18 et GPIO19
- **LED RGB** : Signal → GPIO26
- **Bouton** : Signal → GPIO16 (avec pull-up)

## Configuration logicielle
- MicroPython
- Bibliothèques requises :
  - `machine` (intégrée)
  - `neopixel` (intégrée)
  - `lcd_api` (fichier séparé)
  - `i2c_lcd` (fichier séparé)

## Fonctionnalités
- Détection de niveaux de gaz dangereux
- Surveillance de l'humidité/vapeur
- Activation automatique du ventilateur en cas de détection
- Indications visuelles via LED RGB
- Affichage des données sur écran LCD
- Contrôle manuel via bouton poussoir

## Installation

### 1. Flasher MicroPython sur l'ESP32
Si MicroPython n'est pas déjà installé sur votre ESP32 :
```
esptool.py --port [PORT] erase_flash
esptool.py --port [PORT] --baud 460800 write_flash -z 0x1000 esp32-[VERSION].bin
```

### 2. Installer les bibliothèques nécessaires
Téléchargez les fichiers `lcd_api.py` et `i2c_lcd.py` et transférez-les sur votre ESP32.

### 3. Transférer le code principal
Transférez le fichier `main.py` sur votre ESP32.

## Utilisation
Une fois le code téléchargé sur l'ESP32, le système démarre automatiquement et commence à surveiller l'environnement.

- **LED Rouge** : Niveau de gaz élevé détecté
- **LED Verte** : Niveau d'humidité/vapeur élevé détecté
- **LED Bleue** : État normal, pas de niveaux anormaux détectés
- **Ventilateur** : S'active automatiquement lorsque des niveaux anormaux sont détectés
- **Bouton** : Permet d'activer/désactiver manuellement le système (optionnel)

## Dépannage

### L'écran LCD reste bleu sans affichage de texte
- Vérifiez que l'adresse I2C est correcte (généralement 0x27 ou 0x3F)
- Assurez-vous que les connexions SDA et SCL sont correctes
- Si l'écran a un potentiomètre de réglage du contraste, ajustez-le
- Essayez d'alimenter l'écran en 5V si possible (certains écrans fonctionnent mal en 3.3V)

### Les capteurs ne répondent pas
- Vérifiez les connexions
- Assurez-vous que les broches ADC sont correctement configurées
- Ajustez les seuils de détection en fonction de votre environnement

### Le ventilateur ne s'active pas
- Vérifiez les connexions du ventilateur
- Assurez-vous que la tension est suffisante pour alimenter le ventilateur
- Vérifiez que les broches GPIO sont correctement configurées en sortie

## Personnalisation
Vous pouvez ajuster les seuils de détection en modifiant les valeurs suivantes dans le code :
```python
if gas_level > 3000:  # Modifier cette valeur selon votre capteur et environnement
    # Détection de gaz...

if steam_level > 2000:  # Modifier cette valeur selon votre capteur et environnement
    # Détection de vapeur...
```

## Extensions possibles
- Ajout d'une connectivité WiFi pour la surveillance à distance
- Stockage des données sur une carte SD ou dans le cloud
- Ajout d'alarmes sonores
- Intégration avec d'autres capteurs (température, qualité de l'air, etc.)

## Licence
Ce projet est distribué sous licence libre.

## Contributeurs
- Cabanes Antoine
- Gaspard Virieux
- Langoisco Raphael
- Gruel Leo

## Remerciements
- Keyestudio pour la documentation et les idées de projets
- Communauté MicroPython pour les bibliothèques et le support