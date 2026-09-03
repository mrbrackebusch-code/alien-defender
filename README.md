# Alien Defender

### @explicitHints true

### @preferredEditor blocks

## Introduction

### @showdialog

Earth's defense ship is ready—but its weapons are not. Program **A** to fire one laser, make the minefield and shields stop the alien attack, then turn **B** into the five-pulse burst that can break the mothership shield.

**One ship. Five mines. Three shields. One mothership. Two buttons. Save Earth.**

## 1. Shoot a Laser

Before we do anything we need to get your ship equipped with some weapons. Good thing recent research has developed lasers that can travel through the vacuum of space. **Let's make the “A” button shoot.**

### Add this code

☐ From ``||controller:Controller||``, drag an ``||controller:on [A] button [pressed]||`` container into an empty area of the workspace.

☐ From ``||sprites:Sprites||``, snap a ``||variables(sprites):set [laser] to projectile [ ] from [mySprite] with vx [50] vy [50]||`` block inside the new container.

☐ Open the variable menu at the start of the new block and choose the supplied ``||variables(noclick):laser||`` variable.

☐ Choose `patch_pulse` for the laser's image.

☐ Change ``||variables(noclick):mySprite||`` to ``||variables(noclick):ship||``. Change **vx** to **100** and **vy** to **0**.

### Test your code

☐ Start the game. Move your ship to the alien's height and press **A** once.

### If your code works, you should see

One laser travels to the right from your ship each time you press **A**.

#### ~ tutorialhint

```blocks
let laser: Sprite = null
let ship: Sprite = null
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    laser = sprites.createProjectileFromSprite(assets.image`patch_pulse`, ship, 100, 0)
})
```

## 2. Make a Hit Matter

Nothing happens when your laser hits the alien. Let's change that. **Make the laser and alien disappear when they collide.**

### Add this code

☐ From ``||sprites:Sprites||``, drag an ``||sprites:on [sprite] of kind [Player] overlaps [otherSprite] of kind [Player]||`` event into an empty area of the workspace.

☐ Change the first kind to **Projectile** and the second kind to **Enemy**.

☐ Name the Projectile variable ``||variables(noclick):laser||`` and the Enemy variable ``||variables(noclick):alien||``. These names stand for the exact laser and alien that touched.

☐ Inside the event, add ``||sprites:destroy [mySprite]||``. Choose ``||variables(noclick):laser||``. Add a second destroy block underneath it and choose ``||variables(noclick):alien||``.

### Test your code

☐ Start the game. Move your ship to the alien's height, press **A**, and let the laser hit the alien.

### If your code works, you should see

When the laser touches the alien, the laser disappears and the alien disappears. Then a massive wave of aliens enters the screen.

#### ~ tutorialhint

```blocks
sprites.onOverlap(SpriteKind.Projectile, SpriteKind.Enemy, function (laser, alien) {
    laser.destroy()
    alien.destroy()
})
```

## 3. Wake the Minefield

That first alien was only the beginning. Good thing your allies have already placed five mines in the new wave's path. **Make each mine destroy the alien that touches it.**

### Make this happen

☐ When a sprite of kind **Mine** overlaps another sprite of kind **Enemy**, run code. Name the first sprite **mine** and the second sprite **alien**. Destroy **mine**. Destroy **alien**.

### Test your code

☐ Start the game, defeat the first alien, and watch the wave reach the mines.

### If your code works, you should see

When an alien touches a mine, both disappear in an explosion. Once all five mines have stopped the wave, three shield layers move into place in front of your ship.

#### ~ tutorialhint

```blocks
namespace SpriteKind {
    export const Mine = SpriteKind.create()
}
sprites.onOverlap(SpriteKind.Mine, SpriteKind.Enemy, function (mine, alien) {
    mine.destroy()
    alien.destroy()
})
```

## 4. Make the Shields Defend You

The wave is gone, but alien bolts are already flying toward you. Good thing Earth sent three shield layers to protect your ship. Right now, the bolts pass straight through them. **Make each shield stop one alien bolt.**

### Make this happen

☐ When the alien bolt overlaps the shield, destroy the bolt, then destroy the shield.

### Test your code

☐ Move into the path of the incoming bolts and let your shields take two hits.

### If your code works, you should see

When an alien bolt touches a shield, both disappear. Each hit removes one of the three bright shield marks at the top of the screen. After two hits, your allies send a shield charge across the battlefield.

#### ~ tutorialhint

```blocks
namespace SpriteKind {
    export const AlienBolt = SpriteKind.create()
    export const Shield = SpriteKind.create()
}
sprites.onOverlap(SpriteKind.AlienBolt, SpriteKind.Shield, function (bolt, shieldLayer) {
    bolt.destroy()
    shieldLayer.destroy()
})
```

## 5. Recharge One Layer

Only one shield layer is left. Your allies sent one shield charge to help. **Make the charge rebuild a shield layer when your ship collects it.**

### Make this happen

☐ When your ship overlaps a shield charge, destroy the shield charge and create a sprite of kind **Shield**.

☐ For the new sprite, use ``||variables(sprites):set [shield] to sprite [ ] of kind [Player]||``. Choose the supplied ``||variables(noclick):shield||`` variable, choose the `player_shield` image, and change its kind to **Shield**.

### Test your code

☐ Fly into the moving shield charge.

### If your code works, you should see

When your ship touches the charge, the charge disappears. One new shield layer snaps into the stack in front of your ship, and one bright shield mark comes back. Then the mothership arrives.

#### ~ tutorialhint

```blocks
namespace SpriteKind {
    export const Shield = SpriteKind.create()
    export const ShieldCharge = SpriteKind.create()
}
let shield: Sprite = null
sprites.onOverlap(SpriteKind.Player, SpriteKind.ShieldCharge, function (player, shieldCharge) {
    shieldCharge.destroy()
    shield = sprites.create(assets.image`player_shield`, SpriteKind.Shield)
})
```

## 6. Crack the Mothership Shield

The mothership is here, protected by five shield cells. Your lasers cannot reach it while those cells are in the way. **Make each laser destroy the shield cell it touches.**

### Make this happen

☐ When a laser overlaps a mothership shield, destroy the laser, then destroy the mothership shield.

### Test your code

☐ Line up with the mothership and press **A** once.

### If your code works, you should see

Your laser and one shield cell disappear. Then the mothership repairs the cell. One laser can break one cell, but separate shots are too slow to keep all five down. **The ship needs a special weapon.**

#### ~ tutorialhint

```blocks
namespace SpriteKind {
    export const ShieldCell = SpriteKind.create()
}
sprites.onOverlap(SpriteKind.Projectile, SpriteKind.ShieldCell, function (laser, mothershipShield) {
    laser.destroy()
    mothershipShield.destroy()
})
```

## 7. Build the Five-Pulse Burst

The blocks we've been working with won't be enough. We need more. This time, make the **B-button event** create five lasers instead of just one.

A counted loop repeats the blocks inside it. Put one laser and one short pause inside a loop that repeats **5** times. The loop makes the complete burst without five copied stacks of blocks.

### Add the new pieces

☐ From ``||controller:Controller||``, drag an ``||controller:on [A] button [pressed]||`` container into an empty area of the workspace. Change **A** to **B**.

☐ From ``||loops:Loops||``, put ``||loops:repeat [4] times||`` inside the **B** event and change **4** to **5**.

☐ Inside the repeat block, create the same laser that already works for **A**: supplied variable ``||variables(noclick):laser||``, image `patch_pulse`, source ``||variables(noclick):ship||``, **vx 100**, and **vy 0**.

☐ Directly underneath the laser, still inside the loop, add ``||loops:pause [100] ms||``.

### Predict

Before you test: how many separate lasers should leave the ship after one **B** press?

### Test your code

☐ Return to the mothership, line up with its shield, and press **B** once.

### If your code works, you should see

**A** breaks one shield cell, but it repairs before the next shot arrives. **B** sends a visible line of five laser pulses. The shield cells collapse one after another; the fifth hit destroys the mothership and the game displays **YOU WIN!**

### Help

- If **B** creates nothing, check the ``||controller:on [B] button [pressed]||`` event.
- If **B** creates only one laser, make sure the laser block is inside ``||loops:repeat [5] times||``.
- If five lasers overlap like one, put ``||loops:pause [100] ms||`` directly underneath the laser, inside the loop.
- If the shield repairs, check that the repeat count is **5** and both blocks are inside the loop.

### Explain

How did the words “when the alien bolt overlaps the shield” help you know that you needed an event? How did using a loop reduce the number of blocks needed for the five-pulse burst?

#### ~ tutorialhint

```blocks
let laser: Sprite = null
let ship: Sprite = null
controller.B.onEvent(ControllerButtonEvent.Pressed, function () {
    for (let index = 0; index < 5; index++) {
        laser = sprites.createProjectileFromSprite(assets.image`patch_pulse`, ship, 100, 0)
        pause(100)
    }
})
```

## Finish

### @showdialog

**Congratulations—you completed Alien Defender!**

Move with the direction buttons. Press **A** for one laser. Press **B** for the five-pulse burst. You made the lasers, mines, and shields work, then built the weapon that saved Earth.

```template
namespace SpriteKind {
    export const Mine = SpriteKind.create()
    export const AlienBolt = SpriteKind.create()
    export const Shield = SpriteKind.create()
    export const ShieldCharge = SpriteKind.create()
    export const ShieldCell = SpriteKind.create()
}

let laser: Sprite = null
let shield: Sprite = null
let ship: Sprite = null

function codeAlreadyDoneForYou() {
    laser = ship
    shield = ship
    ship = sprites.create(assets.image`drone_cyan`, SpriteKind.Player)
    ship.setPosition(20, 60)
    ship.setStayInScreen(true)
    controller.moveSprite(ship, 0, 80)
    info.setLife(3)
    scene.setBackgroundImage(assets.image`void_patch_grid`)
    game.setGameOverMessage(true, "YOU WIN!")
    game.setGameOverMessage(false, "TRY AGAIN")
    game.setGameOverEffect(true, effects.confetti)
    game.setGameOverEffect(false, effects.splatter)
    game.setGameOverPlayable(true, music.melodyPlayable(music.powerUp), false)
    game.setGameOverPlayable(false, music.melodyPlayable(music.buzzer), false)
}

codeAlreadyDoneForYou()
```

```customts
namespace SpriteKind {
    export const Mothership = SpriteKind.create()
    export const AlienCannon = SpriteKind.create()
    export const WaveEcho = SpriteKind.create()
    export const ShieldMeter = SpriteKind.create()
    export const WorldEffect = SpriteKind.create()
}

namespace voidPatchGame {
    const PHASE_OPENING = 0
    const PHASE_MINEFIELD = 1
    const PHASE_DEFENSE = 2
    const PHASE_RECHARGE = 3
    const PHASE_BOSS = 4
    const PHASE_VICTORY = 5
    let phase = PHASE_OPENING
    let worldClearing = false
    let victoryStarted = false

    let activeAlien: Sprite = null
    let openingProjectile: Sprite = null
    let openingAlien: Sprite = null
    let openingContactAt = 0

    let mines: Sprite[] = []
    let waveEchoes: Sprite[] = []
    let activeMine: Sprite = null
    let activeWaveAlien: Sprite = null
    let mineContactMine: Sprite = null
    let mineContactAlien: Sprite = null
    let mineContactAt = 0
    let mineDetonations = 0
    let nextWaveAt = 0

    let alienCannon: Sprite = null
    let shieldMeters: Sprite[] = []
    let shieldContactBolt: Sprite = null
    let shieldContactLayer: Sprite = null
    let shieldContactAt = 0
    let shieldHits = 0
    let nextBoltAt = 0

    let shieldCharge: Sprite = null
    let chargeCandidate: Sprite = null
    let chargeBaseline = 0
    let chargeContactAt = 0

    let mothership: Sprite = null
    let shieldCells: Sprite[] = []
    let bossProjectile: Sprite = null
    let bossCell: Sprite = null
    let bossCellIndex = -1
    let bossContactAt = 0
    let bossContactAccepted = false
    let bossHits = 0
    let repairAtByIndex: number[] = [0, 0, 0, 0, 0]

    let recentProjectile: Sprite = null
    let recentProjectileAt = 0
    let recentBolt: Sprite = null
    let recentBoltAt = 0
    let recentShieldLayer: Sprite = null
    let recentShieldLayerAt = 0
    let recentBossCell: Sprite = null
    let recentBossCellAt = 0
    let learnerKindObserversWired = false

    function currentPlayer(): Sprite {
        let players = sprites.allOfKind(SpriteKind.Player)
        return players.length == 1 ? players[0] : null
    }

    function contains(kind: number, target: Sprite): boolean {
        if (target == null) {
            return false
        }
        for (let sprite of sprites.allOfKind(kind)) {
            if (sprite == target) {
                return true
            }
        }
        return false
    }

    function near(first: Sprite, second: Sprite): boolean {
        return first != null && second != null
            && Math.abs(first.x - second.x) <= 14
            && Math.abs(first.y - second.y) <= 14
    }

    function destroyKind(kind: number) {
        for (let sprite of sprites.allOfKind(kind)) {
            sprite.destroy()
        }
    }

    function makeEffect(x: number, y: number, fire: boolean) {
        let effectSprite = sprites.create(assets.image`shield_flash`, SpriteKind.WorldEffect)
        effectSprite.setPosition(x, y)
        effectSprite.setFlag(SpriteFlag.GhostThroughSprites, true)
        effectSprite.lifespan = 220
        if (fire) {
            effectSprite.startEffect(effects.fire, 180)
        } else {
            effectSprite.startEffect(effects.coolRadial, 160)
        }
    }

    function shieldCellImage(index: number): Image {
        if (index == 0) {
            return assets.image`shield_cell_1`
        } else if (index == 1) {
            return assets.image`shield_cell_2`
        } else if (index == 2) {
            return assets.image`shield_cell_3`
        } else if (index == 3) {
            return assets.image`shield_cell_4`
        }
        return assets.image`shield_cell_5`
    }

    function createOpeningAlien() {
        if (phase != PHASE_OPENING || activeAlien != null || openingContactAt != 0 || currentPlayer() == null) {
            return
        }
        activeAlien = sprites.create(assets.image`glitch`, SpriteKind.Enemy)
        activeAlien.setPosition(148, 60)
        activeAlien.setVelocity(-18, 0)
        console.log("ALIEN_DEFENDER phase=opening alien=arrived")
    }

    function updateOpening() {
        createOpeningAlien()
        if (activeAlien != null) {
            if (activeAlien.x <= 94 && activeAlien.vx < 0) {
                activeAlien.vx = 10
            } else if (activeAlien.x >= 132 && activeAlien.vx > 0) {
                activeAlien.vx = -10
            }
        }
        if (openingContactAt != 0) {
            let projectileGone = !contains(SpriteKind.Projectile, openingProjectile)
            let alienGone = !contains(SpriteKind.Enemy, openingAlien)
            if (projectileGone && alienGone) {
                let x = openingAlien.x
                let y = openingAlien.y
                openingProjectile = null
                openingAlien = null
                openingContactAt = 0
                activeAlien = null
                makeEffect(x, y, true)
                beginMinefield()
            } else if (control.millis() - openingContactAt > 700) {
                openingProjectile = null
                openingAlien = null
                openingContactAt = 0
                if (activeAlien == null || !contains(SpriteKind.Enemy, activeAlien)) {
                    activeAlien = null
                }
            }
        }
    }

    function beginMinefield() {
        phase = PHASE_MINEFIELD
        worldClearing = true
        destroyKind(SpriteKind.Enemy)
        worldClearing = false
        activeAlien = null
        mines = []
        waveEchoes = []
        mineDetonations = 0
        nextWaveAt = control.millis() + 350
        for (let index = 0; index < 5; index++) {
            let mine = sprites.create(assets.image`ally_mine`, SpriteKind.Mine)
            mine.setPosition(70, 18 + index * 21)
            mines.push(mine)
        }
        for (let index = 0; index < 6; index++) {
            let echo = sprites.create(assets.image`glitch`, SpriteKind.WaveEcho)
            echo.setPosition(116 + index * 8, 14 + (index % 5) * 22)
            echo.setVelocity(-8, 0)
            echo.setFlag(SpriteFlag.GhostThroughSprites, true)
            waveEchoes.push(echo)
        }
        console.log("ALIEN_DEFENDER phase=minefield mines=5 wave=massive")
    }

    function releaseWaveAlien() {
        if (phase != PHASE_MINEFIELD || activeWaveAlien != null || mineDetonations >= 5 || control.millis() < nextWaveAt) {
            return
        }
        activeMine = mines[mineDetonations]
        if (activeMine == null || !contains(SpriteKind.Mine, activeMine)) {
            return
        }
        activeWaveAlien = sprites.create(assets.image`glitch`, SpriteKind.Enemy)
        activeWaveAlien.setPosition(156, activeMine.y)
        activeWaveAlien.setVelocity(-34, 0)
        console.log("ALIEN_DEFENDER minefield=lane-attack lane=" + mineDetonations)
    }

    function updateMinefield() {
        for (let echo of waveEchoes) {
            if (echo.x < 91) {
                echo.x = 158
                echo.y = 14 + randint(0, 4) * 22
            }
        }
        releaseWaveAlien()
        if (mineContactAt != 0) {
            let mineGone = !contains(SpriteKind.Mine, mineContactMine)
            let alienGone = !contains(SpriteKind.Enemy, mineContactAlien)
            if (mineGone && alienGone) {
                let x = mineContactMine.x
                let y = mineContactMine.y
                mineDetonations += 1
                activeMine = null
                activeWaveAlien = null
                mineContactMine = null
                mineContactAlien = null
                mineContactAt = 0
                nextWaveAt = control.millis() + 360
                makeEffect(x, y, true)
                music.playTone(165 + mineDetonations * 28, 55)
                console.log("ALIEN_DEFENDER mine=legitimate-detonation count=" + mineDetonations)
                if (mineDetonations == 5) {
                    beginShieldDefense()
                }
            } else if (control.millis() - mineContactAt > 700) {
                mineContactMine = null
                mineContactAlien = null
                mineContactAt = 0
            }
        }
        if (activeWaveAlien != null && !contains(SpriteKind.Enemy, activeWaveAlien)) {
            activeWaveAlien = null
            nextWaveAt = control.millis() + 260
        } else if (activeWaveAlien != null && activeWaveAlien.x < 38) {
            activeWaveAlien.setPosition(156, activeMine.y)
            scene.cameraShake(2, 100)
            console.log("ALIEN_DEFENDER minefield=inert-contact-retry")
        }
    }

    function createShieldLayer(): Sprite {
        return sprites.create(assets.image`player_shield`, SpriteKind.Shield)
    }

    function createShieldMeter() {
        shieldMeters = []
        for (let index = 0; index < 3; index++) {
            let meter = sprites.create(assets.image`shield_ui_off`, SpriteKind.ShieldMeter)
            meter.setPosition(8 + index * 8, 8)
            meter.setFlag(SpriteFlag.GhostThroughSprites, true)
            meter.z = 100
            shieldMeters.push(meter)
        }
    }

    function shieldCountNow(): number {
        return sprites.allOfKind(SpriteKind.Shield).length
    }

    function updateShieldStack() {
        let player = currentPlayer()
        let layers = sprites.allOfKind(SpriteKind.Shield)
        if (player != null) {
            for (let index = 0; index < layers.length; index++) {
                layers[index].setPosition(player.x + 13 + index * 6, player.y)
            }
        }
        for (let index = 0; index < shieldMeters.length; index++) {
            if (index < layers.length) {
                shieldMeters[index].setImage(assets.image`shield_ui_on`)
            } else {
                shieldMeters[index].setImage(assets.image`shield_ui_off`)
            }
        }
    }

    function beginShieldDefense() {
        phase = PHASE_DEFENSE
        worldClearing = true
        destroyKind(SpriteKind.Enemy)
        destroyKind(SpriteKind.WaveEcho)
        worldClearing = false
        activeWaveAlien = null
        for (let index = 0; index < 3; index++) {
            createShieldLayer()
        }
        createShieldMeter()
        alienCannon = sprites.create(assets.image`drone_ember`, SpriteKind.AlienCannon)
        alienCannon.setPosition(143, 60)
        alienCannon.setVelocity(0, 7)
        shieldHits = 0
        nextBoltAt = control.millis() + 650
        updateShieldStack()
        console.log("ALIEN_DEFENDER phase=defense physical-shields=3")
    }

    function updateAlienCannon() {
        if (alienCannon == null) {
            return
        }
        if (alienCannon.y < 28) {
            alienCannon.vy = 7
        } else if (alienCannon.y > 92) {
            alienCannon.vy = -7
        }
    }

    function fireAlienBolt(source: Sprite) {
        if (source == null || sprites.allOfKind(SpriteKind.AlienBolt).length != 0) {
            return
        }
        let bolt = sprites.create(assets.image`alien_bolt`, SpriteKind.AlienBolt)
        bolt.setPosition(source.x - 12, source.y)
        bolt.setVelocity(-42, 0)
        bolt.setFlag(SpriteFlag.AutoDestroy, true)
        console.log("ALIEN_DEFENDER bolt=fired")
    }

    function updateDefense() {
        updateAlienCannon()
        updateShieldStack()
        if (control.millis() >= nextBoltAt) {
            fireAlienBolt(alienCannon)
            nextBoltAt = control.millis() + 1500
        }
        if (shieldContactAt != 0) {
            let boltGone = !contains(SpriteKind.AlienBolt, shieldContactBolt)
            let layerGone = !contains(SpriteKind.Shield, shieldContactLayer)
            if (boltGone && layerGone) {
                let x = shieldContactLayer.x
                let y = shieldContactLayer.y
                shieldHits += 1
                shieldContactBolt = null
                shieldContactLayer = null
                shieldContactAt = 0
                makeEffect(x, y, false)
                scene.cameraShake(3, 120)
                console.log("ALIEN_DEFENDER shield=layer-down remaining=" + shieldCountNow())
                if (shieldHits == 2) {
                    beginRecharge()
                }
            } else if (control.millis() - shieldContactAt > 700) {
                shieldContactBolt = null
                shieldContactLayer = null
                shieldContactAt = 0
            }
        }
    }

    function spawnCharge() {
        if (shieldCharge != null && contains(SpriteKind.ShieldCharge, shieldCharge)) {
            return
        }
        shieldCharge = sprites.create(assets.image`shield_charge`, SpriteKind.ShieldCharge)
        shieldCharge.setPosition(20, 14)
        shieldCharge.setVelocity(0, 18)
        console.log("ALIEN_DEFENDER recharge=available")
    }

    function beginRecharge() {
        phase = PHASE_RECHARGE
        destroyKind(SpriteKind.AlienBolt)
        if (alienCannon != null) {
            alienCannon.setVelocity(0, 0)
        }
        chargeCandidate = null
        chargeContactAt = 0
        spawnCharge()
        console.log("ALIEN_DEFENDER phase=recharge physical-shields=" + shieldCountNow())
    }

    function updateRecharge() {
        updateShieldStack()
        if (shieldCharge != null && contains(SpriteKind.ShieldCharge, shieldCharge)) {
            if (shieldCharge.y < 14) {
                shieldCharge.vy = 18
            } else if (shieldCharge.y > 106) {
                shieldCharge.vy = -18
            }
        }
        if (chargeContactAt != 0) {
            let chargeGone = !contains(SpriteKind.ShieldCharge, chargeCandidate)
            let layerCreated = shieldCountNow() == chargeBaseline + 1
            if (chargeGone && layerCreated) {
                chargeCandidate = null
                chargeContactAt = 0
                shieldCharge = null
                updateShieldStack()
                console.log("ALIEN_DEFENDER recharge=legitimate shields=" + shieldCountNow())
                beginBoss(false)
            } else if (control.millis() - chargeContactAt > 800) {
                chargeCandidate = null
                chargeContactAt = 0
                if (chargeGone) {
                    shieldCharge = null
                    spawnCharge()
                }
            }
        }
    }

    function positionBossCells() {
        if (mothership == null) {
            return
        }
        for (let index = 0; index < shieldCells.length; index++) {
            let cell = shieldCells[index]
            if (cell != null && contains(SpriteKind.ShieldCell, cell)) {
                cell.setPosition(mothership.x - 48 + index * 7, mothership.y)
            }
        }
    }

    function createBossCell(index: number): Sprite {
        let cell = sprites.create(shieldCellImage(index), SpriteKind.ShieldCell)
        shieldCells[index] = cell
        return cell
    }

    function armBossRepair(index: number, delay: number) {
        if (index >= 0 && index < 5 && repairAtByIndex[index] == 0) {
            repairAtByIndex[index] = control.millis() + delay
            console.log("ALIEN_DEFENDER boss=cell-repair-armed index=" + index)
        }
    }

    function findUnobservedBossDamage() {
        for (let index = 0; index < shieldCells.length; index++) {
            let cell = shieldCells[index]
            if (cell != null && !contains(SpriteKind.ShieldCell, cell)
                && (bossContactAt == 0 || bossCell != cell)) {
                shieldCells[index] = null
                armBossRepair(index, 360)
            }
        }
    }

    function resetBossShield() {
        destroyKind(SpriteKind.ShieldCell)
        shieldCells = [null, null, null, null, null]
        for (let index = 0; index < 5; index++) {
            createBossCell(index)
        }
        bossHits = 0
        repairAtByIndex = [0, 0, 0, 0, 0]
        positionBossCells()
    }

    function resetPlayerDefense() {
        destroyKind(SpriteKind.Shield)
        for (let index = 0; index < 3; index++) {
            createShieldLayer()
        }
        updateShieldStack()
    }

    function beginBoss(restarting: boolean) {
        phase = PHASE_BOSS
        worldClearing = true
        destroyKind(SpriteKind.AlienBolt)
        destroyKind(SpriteKind.AlienCannon)
        destroyKind(SpriteKind.ShieldCharge)
        destroyKind(SpriteKind.Mothership)
        destroyKind(SpriteKind.ShieldCell)
        worldClearing = false
        alienCannon = null
        shieldCharge = null
        bossProjectile = null
        bossCell = null
        bossCellIndex = -1
        bossContactAt = 0
        bossContactAccepted = false
        if (restarting) {
            info.setLife(3)
            resetPlayerDefense()
        }
        mothership = sprites.create(assets.image`mothership`, SpriteKind.Mothership)
        mothership.setPosition(144, 60)
        mothership.setVelocity(0, 2)
        resetBossShield()
        nextBoltAt = control.millis() + 1100
        console.log("ALIEN_DEFENDER phase=boss state=" + (restarting ? "retry" : "arrived") + " cells=5")
    }

    function indexOfBossCell(cell: Sprite): number {
        for (let index = 0; index < shieldCells.length; index++) {
            if (shieldCells[index] == cell) {
                return index
            }
        }
        return -1
    }

    function beginVictory() {
        if (victoryStarted || mothership == null) {
            return
        }
        victoryStarted = true
        phase = PHASE_VICTORY
        destroyKind(SpriteKind.AlienBolt)
        let defeated = mothership
        mothership = null
        defeated.destroy(effects.fire, 500)
        scene.cameraShake(8, 420)
        music.playTone(523, 80)
        music.playTone(659, 80)
        music.playTone(784, 120)
        pause(480)
        game.gameOver(true)
    }

    function resolveBossContact() {
        if (bossContactAt == 0) {
            return
        }
        let projectileGone = !contains(SpriteKind.Projectile, bossProjectile)
        let cellGone = !contains(SpriteKind.ShieldCell, bossCell)
        if (projectileGone && cellGone) {
            let index = bossCellIndex
            let x = bossCell.x
            let y = bossCell.y
            shieldCells[index] = null
            bossProjectile = null
            bossCell = null
            bossCellIndex = -1
            bossContactAt = 0
            makeEffect(x, y, false)
            if (bossContactAccepted) {
                bossHits += 1
                music.playTone(330 + bossHits * 55, 55)
                console.log("ALIEN_DEFENDER boss=accepted-cell count=" + bossHits)
                if (bossHits == 5) {
                    beginVictory()
                }
            } else {
                armBossRepair(index, 360)
                music.playTone(110, 45)
            }
            bossContactAccepted = false
        } else if (control.millis() - bossContactAt > 800) {
            if (cellGone && bossCellIndex >= 0) {
                shieldCells[bossCellIndex] = null
                armBossRepair(bossCellIndex, 240)
            }
            bossProjectile = null
            bossCell = null
            bossCellIndex = -1
            bossContactAt = 0
            bossContactAccepted = false
        }
    }

    function updateBoss() {
        updateShieldStack()
        if (mothership == null) {
            return
        }
        if (voidPatchDashboard.observedBurstCount() > 0) {
            mothership.vy = 0
        } else if (mothership.y < 30) {
            mothership.vy = 2
        } else if (mothership.y > 90) {
            mothership.vy = -2
        }
        findUnobservedBossDamage()
        positionBossCells()
        if (shieldContactAt != 0) {
            let boltGone = !contains(SpriteKind.AlienBolt, shieldContactBolt)
            let layerGone = !contains(SpriteKind.Shield, shieldContactLayer)
            if (boltGone && layerGone) {
                makeEffect(shieldContactLayer.x, shieldContactLayer.y, false)
                shieldContactBolt = null
                shieldContactLayer = null
                shieldContactAt = 0
            } else if (control.millis() - shieldContactAt > 700) {
                shieldContactBolt = null
                shieldContactLayer = null
                shieldContactAt = 0
            }
        }
        resolveBossContact()
        for (let index = 0; index < repairAtByIndex.length; index++) {
            if (repairAtByIndex[index] != 0 && control.millis() >= repairAtByIndex[index]) {
                let repaired = createBossCell(index)
                repaired.setPosition(mothership.x - 48 + index * 7, mothership.y)
                makeEffect(repaired.x, repaired.y, false)
                console.log("ALIEN_DEFENDER boss=cell-repaired index=" + index)
                repairAtByIndex[index] = 0
            }
        }
        if (control.millis() >= nextBoltAt) {
            fireAlienBolt(mothership)
            nextBoltAt = control.millis() + 1700
        }
    }

    sprites.onOverlap(SpriteKind.Projectile, SpriteKind.Enemy, function (projectile, alien) {
        if (phase == PHASE_OPENING && openingContactAt == 0) {
            openingProjectile = projectile
            openingAlien = alien
            openingContactAt = control.millis()
            console.log("ALIEN_DEFENDER opening=contact-candidate")
        }
    })

    function wireLearnerKindObservers() {
        if (learnerKindObserversWired) {
            return
        }
        learnerKindObserversWired = true

        sprites.onOverlap(SpriteKind.Mine, SpriteKind.Enemy, function (mine, alien) {
            if (phase == PHASE_MINEFIELD && mine == activeMine && alien == activeWaveAlien && mineContactAt == 0) {
                mineContactMine = mine
                mineContactAlien = alien
                mineContactAt = control.millis()
            }
        })

        sprites.onOverlap(SpriteKind.AlienBolt, SpriteKind.Shield, function (bolt, layer) {
            if ((phase == PHASE_DEFENSE || phase == PHASE_BOSS) && shieldContactAt == 0) {
                shieldContactBolt = bolt
                shieldContactLayer = layer
                shieldContactAt = control.millis()
            }
        })

        sprites.onOverlap(SpriteKind.Player, SpriteKind.ShieldCharge, function (player, charge) {
            if (phase == PHASE_RECHARGE && chargeContactAt == 0) {
                chargeCandidate = charge
                chargeBaseline = shieldCountNow()
                chargeContactAt = control.millis()
            }
        })

        sprites.onOverlap(SpriteKind.Projectile, SpriteKind.ShieldCell, function (projectile, cell) {
            if (phase == PHASE_BOSS && bossContactAt == 0) {
                bossProjectile = projectile
                bossCell = cell
                bossCellIndex = indexOfBossCell(cell)
                bossContactAt = control.millis()
                bossContactAccepted = voidPatchDashboard.claimBurstHit(projectile)
            }
        })

        sprites.onOverlap(SpriteKind.Player, SpriteKind.AlienBolt, function (player, bolt) {
            if (phase != PHASE_DEFENSE && phase != PHASE_BOSS) {
                return
            }
            bolt.destroy()
            info.changeLifeBy(-1)
            scene.cameraShake(4, 140)
            music.playTone(131, 70)
            console.log("ALIEN_DEFENDER player=bolt-hit")
        })

        sprites.onDestroyed(SpriteKind.Mine, function (mine) {
            if (phase == PHASE_MINEFIELD && mine == activeMine && mineContactAt == 0
                && activeWaveAlien != null && near(mine, activeWaveAlien)) {
                mineContactMine = mine
                mineContactAlien = activeWaveAlien
                mineContactAt = control.millis()
            }
        })

        sprites.onDestroyed(SpriteKind.AlienBolt, function (bolt) {
            recentBolt = bolt
            recentBoltAt = control.millis()
            if ((phase == PHASE_DEFENSE || phase == PHASE_BOSS) && shieldContactAt == 0
                && recentShieldLayer != null && control.millis() - recentShieldLayerAt <= 160
                && near(bolt, recentShieldLayer)) {
                shieldContactBolt = bolt
                shieldContactLayer = recentShieldLayer
                shieldContactAt = control.millis()
            }
        })

        sprites.onDestroyed(SpriteKind.Shield, function (layer) {
            recentShieldLayer = layer
            recentShieldLayerAt = control.millis()
            if ((phase == PHASE_DEFENSE || phase == PHASE_BOSS) && shieldContactAt == 0
                && recentBolt != null && control.millis() - recentBoltAt <= 160
                && near(recentBolt, layer)) {
                shieldContactBolt = recentBolt
                shieldContactLayer = layer
                shieldContactAt = control.millis()
            }
        })

        sprites.onDestroyed(SpriteKind.ShieldCharge, function (charge) {
            let player = currentPlayer()
            if (phase == PHASE_RECHARGE && chargeContactAt == 0 && player != null && near(player, charge)) {
                chargeCandidate = charge
                chargeBaseline = shieldCountNow()
                chargeContactAt = control.millis()
            }
        })

        sprites.onDestroyed(SpriteKind.ShieldCell, function (cell) {
            recentBossCell = cell
            recentBossCellAt = control.millis()
            if (worldClearing || phase != PHASE_BOSS || bossContactAt != 0) {
                return
            }
            if (recentProjectile != null && control.millis() - recentProjectileAt <= 160 && near(recentProjectile, cell)) {
                let index = indexOfBossCell(cell)
                if (index < 0) {
                    return
                }
                bossProjectile = recentProjectile
                bossCell = cell
                bossCellIndex = index
                bossContactAt = control.millis()
                bossContactAccepted = voidPatchDashboard.claimBurstHit(recentProjectile)
            }
        })
    }

    sprites.onDestroyed(SpriteKind.Projectile, function (projectile) {
        recentProjectile = projectile
        recentProjectileAt = control.millis()
        if (phase == PHASE_BOSS && bossContactAt == 0
            && recentBossCell != null && control.millis() - recentBossCellAt <= 160
            && near(projectile, recentBossCell)) {
            let index = indexOfBossCell(recentBossCell)
            if (index < 0) {
                return
            }
            bossProjectile = projectile
            bossCell = recentBossCell
            bossCellIndex = index
            bossContactAt = control.millis()
            bossContactAccepted = voidPatchDashboard.claimBurstHit(projectile)
        }
    })

    sprites.onDestroyed(SpriteKind.Enemy, function (alien) {
        if (worldClearing) {
            return
        }
        if (phase == PHASE_MINEFIELD && alien == activeWaveAlien && mineContactAt == 0
            && activeMine != null && near(activeMine, alien)) {
            mineContactMine = activeMine
            mineContactAlien = alien
            mineContactAt = control.millis()
        }
        if (phase == PHASE_OPENING && activeAlien == alien && openingContactAt == 0
            && recentProjectile != null && control.millis() - recentProjectileAt <= 160
            && near(recentProjectile, alien)) {
            openingProjectile = recentProjectile
            openingAlien = alien
            openingContactAt = control.millis()
        }
        if (phase == PHASE_OPENING && activeAlien == alien) {
            activeAlien = null
        }
    })

    info.onLifeZero(function () {
        if (phase == PHASE_BOSS && !victoryStarted) {
            beginBoss(true)
        } else if (phase == PHASE_DEFENSE || phase == PHASE_RECHARGE) {
            info.setLife(3)
            destroyKind(SpriteKind.AlienBolt)
            nextBoltAt = control.millis() + 900
        } else if (!victoryStarted) {
            info.setLife(3)
        }
    })

    game.onUpdate(function () {
        wireLearnerKindObservers()
        if (phase == PHASE_OPENING) {
            updateOpening()
        } else if (phase == PHASE_MINEFIELD) {
            updateMinefield()
        } else if (phase == PHASE_DEFENSE) {
            updateDefense()
        } else if (phase == PHASE_RECHARGE) {
            updateRecharge()
        } else if (phase == PHASE_BOSS) {
            updateBoss()
        }
    })

    export function currentPhase(): number {
        return phase
    }

    export function mineDetonationCount(): number {
        return mineDetonations
    }

    export function shieldCount(): number {
        return shieldCountNow()
    }

    export function bossShieldCount(): number {
        return sprites.allOfKind(SpriteKind.ShieldCell).length
    }

    export function acceptedBossHitCount(): number {
        return bossHits
    }
}

namespace voidPatchDashboard {
    let burstProjectiles: Sprite[] = []
    let burstCreatedAt: number[] = []
    let burstClaimed: boolean[] = []
    let burstOriginChecked: boolean[] = []
    let burstStartedAt = 0
    let lastBPressAt = 0
    let burstInvalid = false

    function log(message: string) {
        console.log("ALIEN_DEFENDER " + message)
    }

    function currentPlayer(): Sprite {
        let players = sprites.allOfKind(SpriteKind.Player)
        return players.length == 1 ? players[0] : null
    }

    function startsAtShip(projectile: Sprite): boolean {
        let player = currentPlayer()
        return player != null
            && Math.abs(projectile.x - player.x) <= 20
            && Math.abs(projectile.y - player.y) <= 12
            && projectile.vx > 0
    }

    function resetBurst(startedAt: number) {
        burstProjectiles = []
        burstCreatedAt = []
        burstClaimed = []
        burstOriginChecked = []
        burstStartedAt = startedAt
        burstInvalid = false
        log("burst=window-open")
    }

    function beginBPress() {
        let now = control.millis()
        lastBPressAt = now
        if (burstStartedAt == 0 || now - burstStartedAt > 650 || burstProjectiles.length == 0) {
            resetBurst(now)
        } else {
            log("burst=B-confirmed-after-first-projectile")
        }
    }

    function recordProjectile(projectile: Sprite) {
        let now = control.millis()
        let activeCandidate = burstStartedAt != 0 && burstProjectiles.length > 0 && now - burstStartedAt <= 800
        if (!activeCandidate && !controller.B.isPressed() && now - lastBPressAt > 120) {
            return
        }
        if (burstStartedAt == 0) {
            resetBurst(now)
        } else if (now - burstStartedAt > 800) {
            if (!controller.B.isPressed() && now - lastBPressAt > 120) {
                return
            }
            resetBurst(now)
        }
        if (burstProjectiles.length >= 5) {
            burstInvalid = true
            log("burst=invalid-too-many")
            return
        }
        if (burstCreatedAt.length > 0) {
            let spacing = now - burstCreatedAt[burstCreatedAt.length - 1]
            if (spacing < 60 || spacing > 180) {
                burstInvalid = true
                log("burst=invalid-spacing value=" + spacing)
            }
        }
        burstProjectiles.push(projectile)
        burstCreatedAt.push(now)
        burstClaimed.push(false)
        burstOriginChecked.push(false)
        log("burst=pulse count=" + burstProjectiles.length)
    }

    function verifyOrigins() {
        for (let index = 0; index < burstProjectiles.length; index++) {
            if (!burstOriginChecked[index]) {
                burstOriginChecked[index] = true
                if (!startsAtShip(burstProjectiles[index])) {
                    burstInvalid = true
                    log("burst=invalid-origin")
                }
            }
        }
    }

    export function claimBurstHit(projectile: Sprite): boolean {
        let now = control.millis()
        if (burstInvalid || burstProjectiles.length != 5 || now - burstStartedAt > 2200) {
            log("shield=reject count=" + burstProjectiles.length)
            return false
        }
        if (burstCreatedAt[4] - burstCreatedAt[0] < 240 || burstCreatedAt[4] - burstCreatedAt[0] > 720) {
            log("shield=reject-total-cadence")
            return false
        }
        for (let index = 0; index < burstProjectiles.length; index++) {
            if (burstProjectiles[index] == projectile && !burstClaimed[index] && projectile.vx > 0) {
                burstClaimed[index] = true
                log("shield=accept-pulse index=" + index)
                return true
            }
        }
        log("shield=reject-identity")
        return false
    }

    export function observedBurstCount(): number {
        return burstProjectiles.length
    }

    export function observedBurstValid(): boolean {
        return !burstInvalid && burstProjectiles.length == 5
    }

    sprites.onCreated(SpriteKind.Projectile, recordProjectile)
    controller.B.addEventListener(ControllerButtonEvent.Pressed, beginBPress)
    game.onUpdateInterval(20, verifyOrigins)
    log("observer=ready-invisible")
}
```

```assetjson
{
  "images.g.jres": "{\n    \"*\": {\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"dataEncoding\": \"base64\",\n        \"namespace\": \"myImages\"\n    },\n    \"vp_drone_cyan\": {\n        \"data\": \"hwQQABAAAAAAAABplgAAAAAAkImYCQAAAACZGIGZAAAAkIkREZgJAACZiBERiJkAkIkYERGBmAmZiBEVURGImZmIEVVVEYiZkIkYFVGBmAkAmRgREYGZAACQiBERiAkAAACJERGYAAAAAImIiJgAAAAAkIiICQAAAACQiZgJAAAAAACZmQAAAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"drone_cyan\",\n        \"tags\": []\n    },\n    \"vp_drone_ember\": {\n        \"data\": \"hwQQABAAAAAAAABURQAAAAAAQHRHBAAAAABEEiFEAAAAQCQREUIEAABEIhERIkQAQCQSEREhQgREIhEVUREiREQiEVVVESJEQCQSFVEhQgQARBIRESFEAABAIhERIgQAAAAkERFCAAAAACQiIkIAAAAAQCIiBAAAAABAJEIEAAAAAABERAAAAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"drone_ember\",\n        \"tags\": []\n    },\n    \"vp_glitch\": {\n        \"data\": \"hwQMAAwAAAAAAAqgAAAAAKAgIiICCgAAKqKiKiqiAACgojIjKgoAAAAqozqiAAAAACqqqqIAAAAAKqqqogAAAAAqozqiAAAAoKIyIyoKAAAqoqIqKqIAAKAgIiICCgAAAAAKoAAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"glitch\",\n        \"tags\": []\n    },\n    \"vp_mothership\": {\n        \"data\": \"hwQcABQAAAAAAAAAoAoAAAAAAAAAAACqmqmqAAAAAAAAAKCqmZmqCgAAAAAAAKCZmZmZCgAAAAAAAJqZqZqZqQAAAAAAAJqZqqqZqQAAAAAAoJmqGqGqmQoAAAAAoKmqERGqmgoAAAAAmqkRURURmqkAAAAAmqkRURURmqkAAACgmhoRlVkRoakKAACgmRpRlVkVoZkKAACqmRpVGZFVoZmqAACqmRpVGZFVoZmqAACqmRpVGZFVoZmqAACqmRpVGZFVoZmqAACgmRpRlVkVoZkKAACgmhoRlVkRoakKAAAAmqkRURURmqkAAAAAmqkRURURmqkAAAAAoKmqERGqmgoAAAAAoJmqGqGqmQoAAAAAAJqZqqqZqQAAAAAAAJqZqZqZqQAAAAAAAKCZmZmZCgAAAAAAAKCqmZmqCgAAAAAAAACqmqmqAAAAAAAAAAAAoAoAAAAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"mothership\",\n        \"tags\": []\n    },\n    \"vp_ally_mine\": {\n        \"data\": \"hwQJAAkAAAAAQEQAAAAAAABERQQAAAAAQFRaRAAAAABApaFFAAAAAFSloQUEAAAAQKWhRQAAAABAVFpEAAAAAABERQQAAAAAAEBEAAAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"ally_mine\",\n        \"tags\": []\n    },\n    \"vp_player_shield\": {\n        \"data\": \"hwQFABAAAAAAmZmZmZmZAJCZmZmZmZkJmREREREREZmQmZmZmZmZCQCZmZmZmZkA\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"player_shield\",\n        \"tags\": []\n    },\n    \"vp_shield_charge\": {\n        \"data\": \"hwQJAAkAAAAAkJkAAAAAAACZlQkAAAAAkFlVmQAAAACZVVGVCQAAAJBZVZkAAAAAAJmVCQAAAAAAkJkAAAAAAAAAAAAAAAAAAAAAAAAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"shield_charge\",\n        \"tags\": []\n    },\n    \"vp_shield_ui_on\": {\n        \"data\": \"hwQGAAYAAACQmQkAmRGZABkRkQAZEZEAmRGZAJCZCQA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"shield_ui_on\",\n        \"tags\": []\n    },\n    \"vp_shield_ui_off\": {\n        \"data\": \"hwQGAAYAAADAzAwAzADMAAwAwAAMAMAAzADMAMDMDAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"shield_ui_off\",\n        \"tags\": []\n    },\n    \"vp_shield_cell_1\": {\n        \"data\": \"hwQFAAcAAAAAmQAAkJkJAJkRmQmQmQkAAJkAAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"shield_cell_1\",\n        \"tags\": []\n    },\n    \"vp_shield_cell_2\": {\n        \"data\": \"hwQFAAcAAACQmZkAmZmZCZARkQAAmQkAAJAAAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"shield_cell_2\",\n        \"tags\": []\n    },\n    \"vp_shield_cell_3\": {\n        \"data\": \"hwQFAAcAAAAAmQkAkBmZAJkRkQmQGZkAAJkJAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"shield_cell_3\",\n        \"tags\": []\n    },\n    \"vp_shield_cell_4\": {\n        \"data\": \"hwQFAAcAAAAAkAAAAJkJAJARkQCZmZkJkJmZAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"shield_cell_4\",\n        \"tags\": []\n    },\n    \"vp_shield_cell_5\": {\n        \"data\": \"hwQFAAcAAAAAkAkAAJmZAJkZkQkAmZkAAJAJAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"shield_cell_5\",\n        \"tags\": []\n    },\n    \"vp_alien_bolt\": {\n        \"data\": \"hwQGAAQAAABABAAAREQAAFRFAABURQAAQAQAAEAEAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"alien_bolt\",\n        \"tags\": []\n    },\n    \"vp_shield_flash\": {\n        \"data\": \"hwQJAAkAAAAAmZkJAAAAAJAJAJkAAAAAkAAAkAAAAAAJAAAACQAAAAkAAQAJAAAACQAAAAkAAACQAACQAAAAAJAJAJkAAAAAAJmZCQAAAAA=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"shield_flash\",\n        \"tags\": []\n    },\n    \"vp_patch_pulse\": {\n        \"data\": \"hwQIAAQAAACQCQAAmZkAABmVAAAZlQAAGZUAABmVAACZmQAAkAkAAA==\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"patch_pulse\",\n        \"tags\": []\n    },\n    \"vp_wide_patch\": {\n        \"data\": \"hwQKAAYAAAAAmQAAkJkJAJkRmQAZVZEAGVWRABlVkQAZVZEAmRGZAJCZCQAAmQAA\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"wide_patch\",\n        \"tags\": []\n    },\n    \"vp_void_patch_grid\": {\n        \"data\": \"hwSgAHgAAAD////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////5///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////4//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////+I/4j/iP+I/4j/iP+I/4j/iP+I/4j/iP+I/4j/iP+I/4j/iP+I/4j/iP+I/4j/iP+I/4j/iP+I/4j/iP+ZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZkR8R8R/xHxHxH/EfEfEf8R8R8R/xHxHxH/EfEfEf8R8R8R/xHxHxH/EfEfEf8R8R8R/xHxHxH/EfEfEf+ZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZn/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////6//////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////n/////////////////////////////////////////9f///////////////////////////////P/8//mfnP/8//z//P/8//z//P/8//z//P/8//z//P/8//z/9V9c//z//P/8//z//P/8//z//P/8//////////n/////////////////////////////////////////9f////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////n/////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////9f/////////////////////////////////////////5/////////////////////P/8//z//P/8//z/9V9c//z//P/8//z//P/8//z//P/8//z//P/8//z//P/5n5z//P/8//z//P/8////////////////////9f/////////////////////////////////////////5//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j/////////9f////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////5//////////////////////////////////////////r//////////P/8//z//P/8//z//P/8//z//P/5n5z//P/8//z//P/8//z//P/8//z//P/8//z//P/8//qvrP/8///////////////////////////////5//////////////////////////////////////////r///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////4//////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////+f////////////////////r//////////////////////////////////////////P/8//z//P/8//z/+Z+c//z//P/8//z//P/8//qvrP/8//z//P/8//z//P/8//z//P/8//z//P/8////////////////////+f////////////////////r///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j//////////////////////////////////////////////////////////////////////////////////////////////6////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////j/////////+P/////////4//////////j/////////+P/////////4//////////j/////8=\",\n        \"mimeType\": \"image/x-mkcd-f4\",\n        \"displayName\": \"void_patch_grid\",\n        \"tags\": [\n            \"background\"\n        ]\n    }\n}"
}
```


<!--
ALIEN DEFENDER — PUBLICATION AND SOURCE NOTICE

Alien Defender's game world, vocabulary, sprites, hidden systems, five-overlap progression, and five-pulse ending are original project material. Instructional language and sequencing were adapted from these Microsoft-authored sources:

1. Microsoft MakeCode Arcade, "Galga," pxt-arcade repository, upstream blob a20c99fee74d3b93e9b7c6c4681a7ec18ce1ae14 at repository head 28ed0fb6e9ede7894fcc65687c976316f2ebcf44, captured 2026-08-23.
2. Microsoft MakeCode Arcade, "Dunk," pxt-arcade repository, upstream blob c84243b9beea2fec0ed528d6b801edba5f1bd65d at repository head 28ed0fb6e9ede7894fcc65687c976316f2ebcf44, captured 2026-08-23.
3. Microsoft MakeCode AP CSP, Unit 3 Day 16, makecode-csp repository commit bdfb78e32f41fffba6bebc623379370151123773.

Changes include a new game world, actors, art, hidden runtime, vocabulary, encounter sequence, gradual-release overlap practice, physical shields, repair behavior, and B-button loop finale. Microsoft names identify source provenance only and do not imply endorsement.

The MakeCode AP CSP documentation/content adaptation is provided under CC BY 4.0: https://creativecommons.org/licenses/by/4.0/

MIT License

Copyright (c) Microsoft Corporation. All rights reserved.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
-->
