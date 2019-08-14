<template>
    <div>
        <h3 class="text-center">World of Warcraft Duels</h3>
        <div class="container">

            <div class="card card-body pt-3" v-if="duel">

                <div class="row">
                    <div class="col">
                        <img :src="characters[playerCharacter].iconUrl" class="float-left" alt="">
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
                        <img :src="characters[enemyCharacter].iconUrl" class="float-right" alt="">
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
                        <button @click="playerAbility(0)">{{ characters[playerCharacter].abilities[0].name }}</button>
                        <button @click="playerAbility(1)">{{ characters[playerCharacter].abilities[1].name }}</button>
                        <button @click="playerAbility(2)">{{ characters[playerCharacter].abilities[2].name }}</button>
                    </div>
                </div>

                <div class="row pt-2">
                    <div class="col">
                        <h5>Player's Combat Log</h5>
                        <div class="card card-body" v-if="combatLog" style="height: 200px; overflow-y: scroll">
                            <div v-for="(log, i) in combatLog" :key="i">
                                {{ log }}
                            </div>
                        </div>
                    </div>
                    <div class="col">
                        <h5>Enemy's Combat Log</h5>
                        <div class="card card-body" v-if="combatLog" style="height: 200px; overflow-y: scroll">
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
                    iconUrl: 'https://wow.zamimg.com/images/wow/icons/large/classicon_warrior.jpg',
                    health: 100, rage: 0, mana: 0,
                    physicalArmor: 100, magicArmor: 0,
                    minPhysicalDmg: 5, maxPhysicalDmg: 10,
                    minMagicDmg: 0, maxMagicDmg: 0,
                    abilities: [
                        { name: 'Heroic Strike', minAbilityDmg: 20, maxAbilityDmg: 30, minHeal: 0, maxHeal: 0, rageCost: 25, manaCost: 0,  },
                        { name: 'Whirlwind', minAbilityDmg: 20, maxAbilityDmg: 45, minHeal: 0, maxHeal: 0, rageCost: 35, manaCost: 0, },
                        { name: 'Slam', minAbilityDmg: 15, maxAbilityDmg: 25, minHeal: 0, maxHeal: 0, rageCost: 15, manaCost: 0, },
                    ]
                },
                {
                    name: 'Paladin',
                    iconUrl: 'https://wow.zamimg.com/images/wow/icons/large/classicon_paladin.jpg',
                    health: 100, rage: 0, mana: 100,
                    physicalArmor: 100, magicArmor: 50,
                    minPhysicalDmg: 5, maxPhysicalDmg: 10,
                    minMagicDmg: 0, maxMagicDmg: 0,
                    abilities: [
                        { name: 'Crusader Strike', minAbilityDmg: 20, maxAbilityDmg: 25, minHeal: 0, maxHeal: 0, rageCost: 0, manaCost: 25, },
                        { name: 'Judgement', minAbilityDmg: 25, maxAbilityDmg: 45, minHeal: 0, maxHeal: 0, rageCost: 0, manaCost: 30, },
                        { name: 'Flash Of Light', minAbilityDmg: 0, maxAbilityDmg: 0, minHeal: 15, maxHeal: 30, rageCost: 0, manaCost: 10, },
                    ]
                },
                {
                    name: 'Priest',
                    iconUrl: 'https://wow.zamimg.com/images/wow/icons/large/classicon_priest.jpg',
                    health: 100, rage: 0, mana: 100,
                    physicalArmor: 0, magicArmor: 100,
                    minPhysicalDmg: 1, maxPhysicalDmg: 5,
                    minMagicDmg: 2, maxMagicDmg: 6,
                    abilities: [
                        { name: 'Mind Blast', minAbilityDmg: 15, maxAbilityDmg: 25, minHeal: 0, maxHeal: 0, rageCost: 0, manaCost: 15, },
                        { name: 'Void Bolt', minAbilityDmg: 25, maxAbilityDmg: 35, minHeal: 0, maxHeal: 0, rageCost: 0, manaCost: 40, },
                        { name: 'Shadow Mend', minAbilityDmg: 0, maxAbilityDmg: 0, minHeal: 15, maxHeal: 30, rageCost: 0, manaCost: 10, },
                    ]
                },
                {
                    name: 'Mage',
                    iconUrl: 'https://wow.zamimg.com/images/wow/icons/large/classicon_mage.jpg',
                    health: 100, rage: 0, mana: 100,
                    physicalArmor: 0, magicArmor: 100,
                    minPhysicalDmg: 1, maxPhysicalDmg: 5,
                    minMagicDmg: 2, maxMagicDmg: 6,
                    abilities: [
                        { name: 'Pyro Blast', minAbilityDmg: 20, maxAbilityDmg: 40, minHeal: 0, maxHeal: 0, rageCost: 0, manaCost: 40, },
                        { name: 'Frost Blast', minAbilityDmg: 15, maxAbilityDmg: 25, minHeal: 0, maxHeal: 0, rageCost: 0, manaCost: 20, },
                        { name: 'Arcane Shot', minAbilityDmg: 10, maxAbilityDmg: 20, minHeal: 0, maxHeal: 0, rageCost: 0, manaCost: 15, },
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
            } 
            
            if (enemy.health <= 0) {
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
        playerAbility(index) {
            var player = this.characters[this.playerCharacter]
            var enemy = this.characters[this.enemyCharacter]
            var damage = '' 
            var ability = ''
            var min = ''
            var max = ''

            switch(index) {
                case 0:
                    min = player.abilities[index].minAbilityDmg
                    max = player.abilities[index].maxAbilityDmg
                    ability = player.abilities[index]
                    damage = this.randomizer(min, max)

                    if (player.name == 'Warrior' && player.rage >= ability.rageCost) {
                        enemy.health -= damage
                        player.rage -= ability.rageCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Warrior') {
                        this.combatLog.unshift(`${player.name} needs more rage. Rage required for ${ability.name} is ${ability.rageCost}`)
                        this.enemyAttacks()
                    } else if (player.name == 'Paladin' && player.mana >= ability.manaCost) {
                        enemy.health -= damage
                        player.mana -= ability.manaCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Paladin') {
                        this.combatLog.unshift(`${player.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                        this.enemyAttacks()
                    } else if (player.name == 'Priest' && player.mana >= ability.manaCost) {
                        enemy.health -= damage
                        player.mana -= ability.manaCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Priest') {
                        this.combatLog.unshift(`${player.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                        this.enemyAttacks()
                    } else if (player.name == 'Mage' && player.mana >= ability.manaCost) {
                        enemy.health -= damage
                        player.mana -= ability.manaCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Mage') {
                        this.combatLog.unshift(`${player.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                        this.enemyAttacks()
                    }

                    this.checkWin()
                    break
                case 1: 
                    min = player.abilities[index].minAbilityDmg
                    max = player.abilities[index].maxAbilityDmg
                    ability = player.abilities[index]
                    damage = this.randomizer(min, max)

                    if (player.name == 'Warrior' && player.rage >= ability.rageCost) {
                        enemy.health -= damage
                        player.rage -= ability.rageCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Warrior') {
                        this.combatLog.unshift(`${player.name} needs more rage. Rage required for ${ability.name} is ${ability.rageCost}`)
                        this.enemyAttacks()
                    } else if (player.name == 'Paladin' && player.mana >= ability.manaCost) {
                        enemy.health -= damage
                        player.mana -= ability.manaCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Paladin') {
                        this.combatLog.unshift(`${player.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                        this.enemyAttacks()
                    } else if (player.name == 'Priest' && player.mana >= ability.manaCost) {
                        enemy.health -= damage
                        player.mana -= ability.manaCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Priest') {
                        this.combatLog.unshift(`${player.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                        this.enemyAttacks()
                    } else if (player.name == 'Mage' && player.mana >= ability.manaCost) {
                        enemy.health -= damage
                        player.mana -= ability.manaCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Mage') {
                        this.combatLog.unshift(`${player.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                        this.enemyAttacks()
                    }

                    this.checkWin()
                    break
                case 2:
                    min = player.abilities[index].minAbilityDmg
                    max = player.abilities[index].maxAbilityDmg
                    ability = player.abilities[index]
                    damage = this.randomizer(min, max)

                    if (player.name == 'Warrior' && player.rage >= ability.rageCost) {
                        enemy.health -= damage
                        player.rage -= ability.rageCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Warrior') {
                        this.combatLog.unshift(`${player.name} needs more rage. Rage required for ${ability.name} is ${ability.rageCost}`)
                        this.enemyAttacks()
                    } else if (player.name == 'Paladin' && player.mana >= ability.manaCost) {
                        enemy.health -= damage
                        player.mana -= ability.manaCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Paladin') {
                        this.combatLog.unshift(`${player.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                        this.enemyAttacks()
                    } else if (player.name == 'Priest' && player.mana >= ability.manaCost) {
                        enemy.health -= damage
                        player.mana -= ability.manaCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Priest') {
                        this.combatLog.unshift(`${player.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                        this.enemyAttacks()
                    } else if (player.name == 'Mage' && player.mana >= ability.manaCost) {
                        enemy.health -= damage
                        player.mana -= ability.manaCost
                        this.combatLog.unshift(`${player.name} lands ${ability.name} on ${enemy.name} with ${damage} damage`)
                        this.enemyAttacks()
                    } else if (player.name == 'Mage') {
                        this.combatLog.unshift(`${player.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                        this.enemyAttacks()
                    }

                    this.checkWin()
                    break
                default:
                    return
            }
        },
        enemyAbility(index) {
            var player = this.characters[this.playerCharacter]
            var enemy = this.characters[this.enemyCharacter]
            var damage = '' 
            var ability = ''
            var min = ''
            var max = ''

            switch(index) {
                case 0:
                    min = enemy.abilities[index].minAbilityDmg
                    max = enemy.abilities[index].maxAbilityDmg
                    ability = enemy.abilities[index]
                    damage = this.randomizer(min, max)

                    if (enemy.name == 'Warrior' && enemy.rage >= ability.rageCost) {
                        player.health -= damage
                        enemy.rage -= ability.rageCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Warrior') {
                        this.combatLog.unshift(`${enemy.name} needs more rage. Rage required for ${ability.name} is ${ability.rageCost}`)
                    } else if (enemy.name == 'Paladin' && enemy.mana >= ability.manaCost) {
                        player.health -= damage
                        enemy.mana -= ability.manaCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Paladin') {
                        this.combatLog.unshift(`${enemy.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                    } else if (enemy.name == 'Priest' && enemy.mana >= ability.manaCost) {
                        player.health -= damage
                        enemy.mana -= ability.manaCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Priest') {
                        this.combatLog.unshift(`${enemy.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                    } else if (enemy.name == 'Mage' && enemy.mana >= ability.manaCost) {
                        player.health -= damage
                        enemy.mana -= ability.manaCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Mage') {
                        this.combatLog.unshift(`${enemy.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                    }

                    this.checkWin()
                    break
                case 1: 
                    min = enemy.abilities[index].minAbilityDmg
                    max = enemy.abilities[index].maxAbilityDmg
                    ability = enemy.abilities[index]
                    damage = this.randomizer(min, max)

                    if (enemy.name == 'Warrior' && enemy.rage >= ability.rageCost) {
                        player.health -= damage
                        enemy.rage -= ability.rageCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Warrior') {
                        this.combatLog.unshift(`${enemy.name} needs more rage. Rage required for ${ability.name} is ${ability.rageCost}`)
                    } else if (enemy.name == 'Paladin' && enemy.mana >= ability.manaCost) {
                        player.health -= damage
                        enemy.mana -= ability.manaCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Paladin') {
                        this.combatLog.unshift(`${enemy.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                    } else if (enemy.name == 'Priest' && enemy.mana >= ability.manaCost) {
                        player.health -= damage
                        enemy.mana -= ability.manaCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Priest') {
                        this.combatLog.unshift(`${enemy.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                    } else if (enemy.name == 'Mage' && enemy.mana >= ability.manaCost) {
                        player.health -= damage
                        enemy.mana -= ability.manaCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Mage') {
                        this.combatLog.unshift(`${enemy.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                    }

                    this.checkWin()
                    break
                case 2:
                    min = enemy.abilities[index].minAbilityDmg
                    max = enemy.abilities[index].maxAbilityDmg
                    ability = enemy.abilities[index]
                    damage = this.randomizer(min, max)

                    if (enemy.name == 'Warrior' && enemy.rage >= ability.rageCost) {
                        player.health -= damage
                        enemy.rage -= ability.rageCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Warrior') {
                        this.combatLog.unshift(`${enemy.name} needs more rage. Rage required for ${ability.name} is ${ability.rageCost}`)
                    } else if (enemy.name == 'Paladin' && enemy.mana >= ability.manaCost) {
                        player.health -= damage
                        enemy.mana -= ability.manaCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Paladin') {
                        this.combatLog.unshift(`${enemy.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                    } else if (enemy.name == 'Priest' && enemy.mana >= ability.manaCost) {
                        player.health -= damage
                        enemy.mana -= ability.manaCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Priest') {
                        this.combatLog.unshift(`${enemy.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                    } else if (enemy.name == 'Mage' && enemy.mana >= ability.manaCost) {
                        player.health -= damage
                        enemy.mana -= ability.manaCost
                        this.combatLog.unshift(`${enemy.name} lands ${ability.name} on ${player.name} with ${damage} damage`)
                    } else if (enemy.name == 'Mage') {
                        this.combatLog.unshift(`${enemy.name} needs more mana. Mana required for ${ability.name} is ${ability.manaCost}`)
                    }

                    this.checkWin()
                    break
                default:
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
            var chance = Math.random()
            
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

            if (chance >= 0.75) {
                // Ability 1
                this.enemyAbility(0)
            } else if (chance >= 0.55) {
                // Ability 2
                this.enemyAbility(1)
            } else if (chance >= 0.35) {
                // Ability 3
                this.enemyAbility(2)
            } else {
                return
            }

            return
        }
    }
}
</script>