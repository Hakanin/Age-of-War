# Age of War

Un prototype de jeu "Age of War" développé en HTML, CSS et JavaScript.

## Description

"Age of War" est un jeu de stratégie où les joueurs doivent créer des unités et des tourelles pour attaquer l'ennemi et défendre leur base. Le but du jeu est de détruire la base ennemie tout en protégeant la sienne.

## Fonctionnalités

- Création d'unités et de tourelles
- Attaque automatique des unités et des tourelles
- Système de points de vie pour les unités et les bases
- Gain d'argent en détruisant les unités ennemies
- Augmentation de l'argent au fil du temps
- Contrôles pour créer des unités, mettre en pause et ajuster le volume

## Comment jouer

1. Ouvrez le fichier `age-of-war.html` dans un navigateur web.
2. Utilisez les boutons de contrôle pour créer des unités et mettre le jeu en pause.
3. Surveillez vos points de vie et votre argent pour gérer vos ressources efficacement.
4. Détruisez la base ennemie pour gagner la partie.

## Contrôles

- **Créer une unité (50)** : Crée une unité pour 50 unités d'argent.
- **Pause** : Met le jeu en pause ou reprend le jeu.
- **Volume** : Ajuste le volume de la musique de fond.

## Développement

### Structure du projet

- `age-of-war.html` : Contient le code HTML, CSS et JavaScript pour le jeu.
- `README.md` : Ce fichier.

### Extraits de code

#### Création d'une unité
```javascript
function spawnUnit(type, isBot = false) {
    const unitCost = 50;
    const warning = document.getElementById('warning');
    const currentTime = Date.now();

    if (currentTime - lastUnitSpawnTime < 1000) {
        return;
    }

    if (type === 'player' && playerMoney >= unitCost) {
        playerMoney -= unitCost;
        updateMoneyDisplay();
        warning.style.visibility = 'hidden';
        clearTimeout(warningTimeout);
        lastUnitSpawnTime = currentTime;
    } else if (type === 'player' && playerMoney < unitCost) {
        clearTimeout(warningTimeout);
        warning.style.visibility = 'visible';
        warningTimeout = setTimeout(() => {
            warning.style.visibility = 'hidden';
        }, 3000);
        return;
    }

    const y = 200;
    const x = type === 'player' ? 0 : canvas.width - 20;
    units.push(new Unit(x, y, type, isBot));
}

#### Boucle de jeu
function gameLoop() {
    context.clearRect(0, 0, canvas.width, canvas.height);

    turrets.forEach(turret => {
        turret.draw();
        turret.attack();
    });

    units.forEach(unit => {
        const closeEnemies = units.filter(otherUnit => 
            unit !== otherUnit &&
            unit.isColliding(otherUnit) &&
            unit.isBot !== otherUnit.isBot
        );

        if (closeEnemies.length > 0) {
            unit.attack(closeEnemies[0]);
        } else {
            unit.move();
        }

        unit.draw();
    });

    updateBaseHPBars();

    requestAnimationFrame(gameLoop);
}
Auteur
Développé par Hakan INAN.

Licence
Ce projet est sous licence MIT. Voir le fichier LICENSE pour plus de détails.