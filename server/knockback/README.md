# Knockback Calculations
Specifically within the player calculations, there are calculations related to the knockback a player would receive upon being hit with a weapon:
<br>
```const te = .3 * (P.weightM || 1) + (c.weapons[this.weaponIndex].knock || 0);```
<br><br>
in this case, `(P.weightM || 1)` would be equivalent to 35 (player scale)

## Knockback values for weapons
The `.knock` values for weapons can be found below:

```
katana: 0.15,
polearm: 0.2,
bat: 0.7,
daggers: 0.1
```
