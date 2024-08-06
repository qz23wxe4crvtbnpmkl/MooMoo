# Projectile prediction
This involves predicting the next position of the projectile. This is useful for a multitude of things such as Damage Potential (insert link to it later).

## Projectile Distance Per Tick Mappings
Below are the Projectiles Distance Per Tick values (in order with the Projectiles Info Array):
```
const projDistancePerTick = [1.6, 1.6, 2.5, 2, NaN, 3.6].map(e => e * (1e3/9));
```
