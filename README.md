# CHROMIUMDISPLAY3 🚀

`CHROMIUMDISPLAY3` est un add-on Home Assistant OS (HAOS) ultra-léger et hautement optimisé, conçu spécifiquement pour propulser des dalles tactiles et des affichages de kiosques interactifs. 

Contrairement aux solutions traditionnelles lourdes basées sur X11 (Openbox) ou des architectures Wayland instables (Weston/Cog), cet add-on utilise **Chromium en mode natif avec la plateforme Ozone/DRM**. Il communique directement avec le GPU de la machine et le noyau Linux (`seatd`/`udev`), offrant une réactivité tactile instantanée et une fluidité maximale pour l'affichage des flux de caméras en direct, tout en maintenant une consommation CPU et RAM proche de zéro.

---

## ✈️ CHECK-LIST DE DÉCOLLAGE (SÉQUENCE DE BOOT)

Au démarrage de l'add-on, une routine d'auto-diagnostic inspecte le système à la manière d'une check-list de cockpit d'avion. Vous pouvez suivre ce listing en temps réel dans les journaux (logs) :

* **`[ OK ] Étape 1 : Gestionnaire d'accès seatd opérationnel.`** -> La socket de contrôle matériel est créée pour allouer les droits d'accès au GPU sans privilèges root dégradés.
* **`[ OK ] Étape 2 : Initialisation de UDEV pour le pavé tactile...`** -> Les pilotes d'entrées génériques (`evdev`) scannent et lient les dalles tactiles multipoints pour assurer un défilement et une interaction sans latence.
* **`[ OK ] Étape 3 : Détection de l'accélérateur graphique...`** -> Le bus PCI valide la présence de votre carte graphique (ex: Intel UHD Graphics 600) et monte les nœuds de rendu DRM (`/dev/dri/card0` et `renderD128`) pour activer l'accélération matérielle.

---

## 🔐 CONNEXION AUTOMATIQUE (TRUSTED NETWORKS)

Pour que votre dalle tactile se connecte instantanément à votre tableau de bord sans jamais demander de nom d'utilisateur ni de mot de passe au démarrage, nous exploitons le système des **Réseaux de confiance (Trusted Networks)** de Home Assistant.

### Configuration côté Home Assistant

1. Ouvrez votre fichier de configuration principal de Home Assistant (`configuration.yaml`).
2. Ajoutez ou modifiez la section `auth_providers` pour autoriser le conteneur de l'add-on (ou l'IP de votre dalle si elle est déportée) à se connecter automatiquement. 

*Exemple de configuration standard :*

```yaml
homeassistant:
  auth_providers:
    - type: trusted_networks
      trusted_networks:
        - 127.0.0.1
        - ::1
        - 172.30.32.0/23  # Plage IP interne obligatoire des add-ons Home Assistant
      trusted_users:
        172.30.32.0/23: TON_USER_ID_HA
    - type: homeassistant
