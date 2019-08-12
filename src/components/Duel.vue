<template>
    <div>
        <h3 class="text-center">This is the Duel Component</h3>
        <div class="container">

            <div class="card card-body pt-3" v-if="duel">

                <div class="row">
                    <div class="col">
                        <div class="text-center">{{ characters[playerCharacter].name }}</div>
                        <div class="progress rounded-0">
                            <div class="progress-bar bg-success" role="progressbar" 
                            style="width: 100%" :style="{width: characters[playerCharacter].health + '%'}" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100">
                            {{ characters[playerCharacter].health + '%' }}
                            </div>
                        </div>
                        <div class="progress rounded-0">
                            <div v-if="characters[playerCharacter].name == 'Warrior'" class="progress-bar bg-danger" role="progressbar" 
                            style="width: 0%" :style="{width: characters[playerCharacter].rage + '%'}" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100">
                            {{ characters[playerCharacter].rage + '%' }}
                            </div>
                            <div v-else class="progress-bar bg-primary" role="progressbar" 
                            style="width: 0%" :style="{width: characters[playerCharacter].mana + '%'}" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100">
                            {{ characters[playerCharacter].mana + '%' }}
                            </div>
                        </div>
                    </div>

                    <div class="col">
                        <div class="text-center">{{ characters[enemyCharacter].name }}</div>
                        <div class="progress rounded-0">
                            <div class="progress-bar bg-success" role="progressbar" 
                            style="width: 100%" :style="{width: characters[enemyCharacter].health + '%'}" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100">
                            {{ characters[enemyCharacter].health + '%' }}
                            </div>
                        </div>
                        <div class="progress rounded-0">
                            <div v-if="characters[enemyCharacter].name == 'Warrior'" class="progress-bar bg-danger" role="progressbar" 
                            style="width: 0%" :style="{width: characters[enemyCharacter].rage + '%'}" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100">
                            {{ characters[enemyCharacter].rage + '%' }}
                            </div>
                            <div v-else class="progress-bar bg-primary" role="progressbar" 
                            style="width: 0%" :style="{width: characters[enemyCharacter].mana + '%'}" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100">
                            {{ characters[enemyCharacter].mana + '%' }}
                            </div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col text-center pt-3">
                        <button @click="attack()">Normal Attack</button>
                        <button @click="startGame()">Restart</button>
                        <button @click="endGame()">Forfeit</button>
                        <br>
                        <button @click="ability(1)">{{ characters[playerCharacter].abilities[0].name }}</button>
                        <button @click="ability(2)">{{ characters[playerCharacter].abilities[1].name }}</button>
                        <button @click="ability(3)">{{ characters[playerCharacter].abilities[2].name }}</button>
                    </div>
                </div>

                <div class="row">
                    <div class="col">
                        <h4>Combat Log</h4>
                        <div class="card card-body" v-if="combatLog" style="height: 100px; overflow-y: scroll">
                            <div v-for="(log, i) in combatLog" :key="i">
                                {{ log }}
                            </div>
                        </div>
                    </div>
                </div>

            </div>

            <div class="card card-body pt-3" v-else>
                <div>Select a character</div>
                <ul>
                    <li v-for="(character, i) in characters" :key="i">
                        {{ character.name }} - <button @click="characterSelected(i)">Select</button>
                    </li>
                </ul>
            </div>

        </div>
    </div>
</template>

<script>
export default {
    name: 'Duel',
    data() {
        return {
            duel: false,
            playerCharacter: null,
            enemyCharacter: null,
            combatLog: [],
            characters: [
                {
                    name: 'Warrior',
                    health: 100, rage: 0, mana: 0,
                    physicalArmor: 100, magicArmor: 0,
                    minPhysicalDmg: 10, maxPhysicalDmg: 30,
                    minMagicDmg: 0, maxMagicDmg: 0,
                    abilities: [
                        { name: 'Heroic Strike', minAbilityDmg: 20, maxAbilityDmg: 45, minHeal: 0, maxHeal: 0, },
                        { name: 'Whirlwind', minAbilityDmg: 20, maxAbilityDmg: 45, minHeal: 0, maxHeal: 0, },
                        { name: 'Slam', minAbilityDmg: 20, maxAbilityDmg: 45, minHeal: 0, maxHeal: 0, },
                    ]
                },
                {
                    name: 'Paladin',
                    health: 100, rage: 0, mana: 100,
                    physicalArmor: 100, magicArmor: 50,
                    minPhysicalDmg: 5, maxPhysicalDmg: 20,
                    minMagicDmg: 3, maxMagicDmg: 15,
                    abilities: [
                        { name: 'Crusader Strike', minAbilityDmg: 20, maxAbilityDmg: 45, minHeal: 0, maxHeal: 0, },
                        { name: 'Judgement', minAbilityDmg: 20, maxAbilityDmg: 45, minHeal: 0, maxHeal: 0, },
                        { name: 'Flash Of Light', minAbilityDmg: 0, maxAbilityDmg: 0, minHeal: 10, maxHeal: 30, },
                    ]
                },
                {
                    name: 'Priest',
                    health: 100, rage: 0, mana: 100,
                    physicalArmor: 0, magicArmor: 100,
                    minPhysicalDmg: 1, maxPhysicalDmg: 10,
                    minMagicDmg: 15, maxMagicDmg: 30,
                    abilities: [
                        { name: 'Priest Ability #1', minAbilityDmg: 20, maxAbilityDmg: 45, minHeal: 0, maxHeal: 0, },
                        { name: 'Priest Ability #2', minAbilityDmg: 20, maxAbilityDmg: 45, minHeal: 0, maxHeal: 0, },
                        { name: 'Priest Ability #3', minAbilityDmg: 0, maxAbilityDmg: 0, minHeal: 10, maxHeal: 30, },
                    ]
                },
                {
                    name: 'Mage',
                    health: 100, rage: 0, mana: 100,
                    physicalArmor: 0, magicArmor: 100,
                    minPhysicalDmg: 1, maxPhysicalDmg: 10,
                    minMagicDmg: 15, maxMagicDmg: 30,
                    abilities: [
                        { name: 'Fire Ball', minAbilityDmg: 25, maxAbilityDmg: 65, minHeal: 0, maxHeal: 0, },
                        { name: 'Frost Ball', minAbilityDmg: 25, maxAbilityDmg: 65, minHeal: 0, maxHeal: 0, },
                        { name: 'Arcane Shot', minAbilityDmg: 10, maxAbilityDmg: 55, minHeal: 0, maxHeal: 0, },
                    ]
                },
            ]
        }
    },
    methods: {
        characterSelected(index) {
            var maxLength = this.characters.length,
            minLength = 0
            var randomIndex = this.randomizer(minLength, maxLength)
            if (randomIndex == index) {
                randomIndex = this.randomizer(minLength, maxLength)
            }
            this.playerCharacter = index
            this.enemyCharacter = randomIndex
            this.startGame()
        },
        randomizer(min, max) {
            return Math.round(Math.floor(Math.random() * (+max - +min)) + +min)
        },
        startGame() {
            this.duel = true
            this.initialState()
        },
        endGame() {
            this.duel = false
            this.initialState()
        },
        checkWin() {
            var player = this.characters[this.playerCharacter]
            var enemy = this.characters[this.enemyCharacter]
            if (player.health <= 0) {
                if (confirm('You have lost! Try again?')) {
                    return this.startGame()
                }
            } else if (enemy.health <= 0) {
                if (confirm('You won! Play again?')) {
                    return this.startGame()
                }
            }
        },
        initialState() {
            var player = this.characters[this.playerCharacter]
            var enemy = this.characters[this.enemyCharacter]
            this.combatLog = []
            player.health = 100, player.rage = 0, player.mana = 100
            enemy.health = 100, enemy.rage = 0, enemy.mana = 100
        },
        ability(index) {
            var player = this.characters[this.playerCharacter]
            var enemy = this.characters[this.enemyCharacter]
            var damage = ''

            switch(index) {
                case 1:
                    this.checkWin()
                    damage = this.randomizer(player.abilities[0].minAbilityDmg, player.abilities[0].maxAbilityDmg)
                    if (player.name == 'Warrior' && player.rage > 30) {
                        enemy.health -= damage
                        player.rage -= 30
                        this.combatLog.unshift(`${player.name} lands ${player.abilities[0].name} on ${enemy.name} with ${damage} damage`)
                    } else if (player.name == 'Warrior') {
                        this.combatLog.unshift(`${player.name} needs more rage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Paladin' && player.mana > 30) {
                        enemy.health -= damage
                        player.mana -= 30
                        this.combatLog.unshift(`${player.name} lands ${player.abilities[0].name} on ${enemy.name} with ${damage} damage`)
                    }
                    break
                case 2: 
                    this.checkWin()
                    damage = this.randomizer(player.abilities[1].minAbilityDmg, player.abilities[1].maxAbilityDmg)
                    enemy.health -= damage
                    this.combatLog.unshift(`${player.name} lands ${player.abilities[1].name} on ${enemy.name}`)
                    break
                case 3:
                    this.checkWin()
                    damage = this.randomizer(player.abilities[2].minAbilityDmg, player.abilities[2].maxAbilityDmg)
                    enemy.health -= damage
                    this.combatLog.unshift(`${player.name} lands ${player.abilities[2].name} on ${enemy.name}`)
                    break
                default:
                    this.checkWin()
                    return
            }
        },
        attack() {
            this.playerAttacks()
            this.enemyAttacks()
            this.checkWin()
        },
        playerAttacks() {
            var player = this.characters[this.playerCharacter]
            var enemy = this.characters[this.enemyCharacter]
            var playerPhysicalDmg = this.randomizer(player.minPhysicalDmg, player.maxPhysicalDmg)
            var playerMagicDmg = this.randomizer(player.minMagicDmg, player.maxMagicDmg)
            
            if (player.name == 'Warrior') {
                this.combatLog.unshift(`The ${player.name} hits ${enemy.name} with ${playerPhysicalDmg} of physical damage`)
                enemy.health -= playerPhysicalDmg
                player.rage += playerPhysicalDmg
            } else if (player.name == 'Paladin') {
                this.combatLog.unshift(`The ${player.name} hits ${enemy.name} with ${playerPhysicalDmg} of physical damage`)
                enemy.health -= playerPhysicalDmg
            } else if (player.name == 'Priest') {
                this.combatLog.unshift(`The ${player.name} hits ${enemy.name} with ${playerMagicDmg} of magical damage`)
                enemy.health -= playerMagicDmg
            } else if (player.name == 'Mage') {
                this.combatLog.unshift(`The ${player.name} hits ${enemy.name} with ${playerMagicDmg} of magical damage`)
                enemy.health -= playerMagicDmg
            }
            return
        },
        enemyAttacks() {
            var player = this.characters[this.playerCharacter]
            var enemy = this.characters[this.enemyCharacter]
            var enemyPhysicalDmg = this.randomizer(enemy.minPhysicalDmg, enemy.maxPhysicalDmg)
            var enemyMagicDmg = this.randomizer(enemy.minMagicDmg, enemy.maxMagicDmg)
            
            if (enemy.name == 'Warrior') {
                this.combatLog.unshift(`The ${enemy.name} hits ${player.name} with ${enemyPhysicalDmg} of physical damage`)
                player.health -= enemyPhysicalDmg
                enemy.rage += enemyPhysicalDmg
            } else if (enemy.name == 'Paladin') {
                this.combatLog.unshift(`The ${enemy.name} hits ${player.name} with ${enemyPhysicalDmg} of physical damage`)
                player.health -= enemyPhysicalDmg
            } else if (enemy.name == 'Priest') {
                this.combatLog.unshift(`The ${enemy.name} hits ${player.name} with ${enemyMagicDmg} of magical damage`)
                player.health -= enemyMagicDmg
            } else if (enemy.name == 'Mage') {
                this.combatLog.unshift(`The ${enemy.name} hits ${player.name} with ${enemyMagicDmg} of magical damage`)
                player.health -= enemyMagicDmg
            }
            return
        }
    }
}
</script>