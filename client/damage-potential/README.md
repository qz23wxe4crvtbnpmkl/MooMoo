# What is damage potential?
Damage potential involves calculating how much damage an enemy can do. This is most noticeably used in heal.
In this docs, we will cover one of the more simple damage potential techniques.

# Why?
Damage potential allows you to leak shame after an insta-kill. This makes it widely popular for healing (despite it being in majority of mods today, people still don't seem fond of it).

# How?
Lets first start off by making a class for our damage potential:
```
class DamagePotential {
    static getPrimaryDamage(enemy) {

    }

    static getSecondaryDamage(enemy) {

    }

    static find(enemy) {

    }
}
```

In the code block above, we have made a class `DamagePotential`, with the `getPrimaryDamage`, `getSecondaryDamage`, and `find` methods.

Lets start by calculating the primary damage of an enemy:
```
static getPrimaryDamage(enemy) {
    var soleDamage = R.weapons[enemy.primaryIndex].dmg; // This is the Damage without any variants.
    soleDamage *= T.weaponVariants[tmpObj.primaryVariant].val; // We account for weapon variants.

    if(enemy.skinIndex == 7) {
        soleDamage *= 1.5; // Account for Bull Hat
    } else if (enemy.skinIndex == 55) {
        soleDamage *= 1.2; // Account for Blood Thirster
    }

    return soleDamage;
}
```
In the above code block, we calculate the enemy's weapon damage, including variants and hats that deal extra damage.

Now that we have defined our `getPrimaryDamage` method, lets move on to the `getSecondaryDamage`.
```
static getSecondaryDamage(enemy) {
    if(!enemy.secondaryIndex || enemy.primaryIndex == 0) return;
    var soleDamage = 0;
    if(enemy.secondaryIndex == 10) { // Check if they have Great Hammer (since damage calculations using Hammer are different from porjectile weapons)
        soleDamage = R.weapons[enemy.secondaryIndex].dmg; // Great Hammer base damage.
        soleDamage *= T.weaponVariants[tmpObj.secondaryVariant].val; // Account for variants.
    } else { // This means they have a projectile weapon (e.x. musket, bow, etc)
        soleDamage = R.weapons[enemy.secondaryIndex].Pdmg; //Note: Pdmg is not natively found in the bundle, you can find the damages for each weapon [addlinkherelater](https://microsoft.com)
        // Since projectiles damage don't change from variants, we don't need to account for them
    }

    return soleDamage
}
```
In the above code block, we check to see if enemy has Great Hammer (because calculations are done different for non-projectile weapons), otherwise we do it for projectile damage.
<br>**Note: `.Pdmg` instance is not natively found in the bundle.**

Now lets combine these methods into the main `find` method:
```
static find(enemy, player) {
    var potentialDamage = DamagePotential.getPrimaryDamage(enemy) + DamagePotential.getSecondaryDamage(enemy) + (enemy.turretReload == 0 && player.skinIndex != 22 ? 25 : 0); // Turret Reload logic may vary, double check.

    if(player.skinIndex == 6) {
        potentialDamage *= 0.75; // If the player has soldier, reduce dmg by 75%.
    }

    return potentialDamage;
}
```

And you're done! To utilise the new Damage Potential, simply call `DamagePotential.find(enemy, player)`.
