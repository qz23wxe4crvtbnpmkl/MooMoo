# What is preplace?
Preplace was born out of the creative genius of Watersheep, a brilliant young mind from Korea. Prior to its conception, many had speculated about the possibility of such a revolutionary innovation, but few believed it could be achieved. Undeterred by the skepticism, Watersheep poured his heart and soul into bringing his vision to life. Through countless hours of intense focus and unwavering dedication, he successfully crafted a masterpiece that would shatter expectations and push the boundaries of what was thought possible. With Preplace, Watersheep has proven that even the most ambitious ideas can be transformed into reality, inspiring a new generation of innovators and redefining the landscape of MooMoo.io

## Idea behind preplace
The idea behind preplace (at least the one I am showcasing in this docs) is to place as fast as possible when the next Tick happens.
The reason you can't put this in `function _i` (or `function updatePlayers`) is because there is a delay from the server to your client (thats the ping the game shows at the top).
Preplace is about shaving off as much of that delay as possible.

## How to implement it?
1. We must first track the time since the last Tick.
Example using Pixel's updatePlayers.
```
var TimeSinceTick = 0;
function updatePlayers(data) {
  TimeSinceTick = Date.now();
}
```

2. Next, we will do some calculations within `function Of()` (or `function updateGame()`).

We will do our calculations inside the player loop, which looks like this in bundle:
```
        for (var t = 0; t < Game.players.length + ye.length; ++t)
            if (y = Game.players[t] || ye[t - Game.players.length],
                y.visible) {
```
And this in Pixel's bundle:
```
    for (let i = 0; i < players.length + ais.length; ++i) {
        tmpObj = players[i] || ais[i - players.length];
        if (tmpObj.visible) {
```

3. Make sure to add a condition to check if y/tmpObj is a player (through `y.isPlayer` or `tmpObj.isPlayer`).

4. Next, we will calculate the amount of damage the enemy can deal.

`var potentialDamage = items.weapons[tmpObj.weaponIndex].dmg * (config.weaponVariants[tmpObj[(index < 9 ? "prima" : "seconda") + "ryVariant"]].val) * (items.weapons[index].sDmg || 1) * (tmpObj.skinIndex == 40 ? 3.3 : 1);`<br><br>
Breakdown:<br>
  · `items.weapons[tmpObj.weaponIndex].dmg` - Retrieves default weapon damage.<br>
  · `config.weaponVariants[tmpObj[(index < 9 ? "prima" : "seconda") + "ryVariant"]].val` - Accounts for Weapon Variants (tailored to Pixel's bundle).<br>
  · `(items.weapons[index].sDmg || 1)` - Accounts for extra building damage (differs from damage you deal to entities). **Currently only affects Great Hammer**.<br>
  · `(tmpObj.skinIndex == 40 ? 3.3 : 1)` - Accounts for extra damage dealt by Tank Gear.<br>
