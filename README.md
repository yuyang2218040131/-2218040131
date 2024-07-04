import random

class Field:
    def __init__(self, name):
        self.name = name
        self.field_type = None
        
    def changeField(self):
        # Randomly choose one of the three field types
        field_types = ["Toxic Wasteland", "Healing Meadows", "Castle Walls"]
        self.field_type = random.choice(field_types)
        
    def applyEffect(self, player1, player2):
        if self.field_type == "Toxic Wasteland":
            print("Field type: Toxic Wasteland - damages both combatants for 5 health each.")
            player1.takeDamage(5)
            player2.takeDamage(5)
        elif self.field_type == "Healing Meadows":
            print("Field type: Healing Meadows - heals both combatants for 5 health each.")
            player1.heal(5)
            player2.heal(5)
        elif self.field_type == "Castle Walls":
            print("Field type: Castle Walls - no effects.")
        else:
            print("Invalid field type.")
            
# Example Player class for illustration purposes
class Player:
    def __init__(self, name, health):
        self.name = name
        self.health = health
        self.max_health = 100  # Assuming max health is 100
        
    def takeDamage(self, amount):
        self.health -= amount
        
    def heal(self, amount):
        self.health += amount
        # Healing can exceed max health
        # Uncomment the following lines if you want to limit max health
        # if self.health > self.max_health:
        #     self.health = self.max_health
        
    def __str__(self):
        return f"{self.name} (Health: {self.health}/{self.max_health})"

# Example usage:
if __name__ == "__main__":
    # Create players
    player1 = Player("Player 1", 80)
    player2 = Player("Player 2", 90)
    
    # Create a Field instance
    field = Field("Battlefield")
    
    # Change the field type randomly
    field.changeField()
    
    # Apply the effect of the current field type
    field.applyEffect(player1, player2)
    
    # Display players' status after the effect
    print(player1)
    print(player2)
import random

class Player:
    def __init__(self, name, health):
        self.name = name
        self.health = health
        self.max_health = 100  # Assuming max health is 100
        
    def takeDamage(self, amount):
        self.health -= amount
        if self.health < 0:
            self.health = 0
        
    def heal(self, amount):
        self.health += amount
        if self.health > self.max_health:
            self.health = self.max_health
        
    def isAlive(self):
        return self.health > 0
        
    def __str__(self):
        return f"{self.name} (Health: {self.health}/{self.max_health})"

class Field:
    def __init__(self, name):
        self.name = name
        self.field_type = None
        
    def changeField(self):
        field_types = ["Toxic Wasteland", "Healing Meadows", "Castle Walls"]
        self.field_type = random.choice(field_types)
        
    def applyEffect(self, player1, player2):
        if self.field_type == "Toxic Wasteland":
            player1.takeDamage(5)
            player2.takeDamage(5)
        elif self.field_type == "Healing Meadows":
            player1.heal(5)
            player2.heal(5)
        elif self.field_type == "Castle Walls":
            pass  # No effects
        else:
            print("Invalid field type.")
            
class Arena:
    def __init__(self, name):
        self.name = name
        self.combatants = []
        self.field = Field("Battlefield")
        
    def addCombatant(self, combatant):
        if combatant not in self.combatants:
            self.combatants.append(combatant)
        else:
            print(f"{combatant.name} is already in the arena.")
            
    def removeCombatant(self, combatant):
        if combatant in self.combatants:
            self.combatants.remove(combatant)
        else:
            print(f"{combatant.name} is not in the arena.")
            
    def listCombatants(self):
        print(f"Combatants in Arena '{self.name}':")
        for combatant in self.combatants:
            print(combatant)
            
    def restoreCombatants(self):
        for combatant in self.combatants:
            combatant.health = combatant.max_health
            
    def duel(self):
        # Check if there are at least two valid combatants with health
        valid_combatants = [combatant for combatant in self.combatants if combatant.isAlive()]
        if len(valid_combatants) < 2:
            print("Not enough valid combatants to duel.")
            return
        
        # Randomly select two combatants
        fighter1, fighter2 = random.sample(valid_combatants, 2)
        
        round_count = 0
        while round_count < 10 and fighter1.isAlive() and fighter2.isAlive():
            round_count += 1
            print(f"\nRound {round_count}:")
            
            # Apply field effect
            self.field.changeField()
            self.field.applyEffect(fighter1, fighter2)
            
            # Fighter 1 attacks Fighter 2
            if fighter1.isAlive():
                damage = random.randint(10, 20)  # Assume random damage between 10 to 20
                fighter2.takeDamage(damage)
                print(f"{fighter1.name} attacks {fighter2.name} for {damage} damage.")
            
            # Fighter 2 attacks Fighter 1
            if fighter2.isAlive():
                damage = random.randint(10, 20)  # Assume random damage between 10 to 20
                fighter1.takeDamage(damage)
                print(f"{fighter2.name} attacks {fighter1.name} for {damage} damage.")
                
        # Determine winner
        if fighter1.isAlive() and fighter2.isAlive():
            print("\nThe duel ends in a draw!")
        elif fighter1.isAlive():
            print(f"\n{fighter1.name} wins the duel!")
        else:
            print(f"\n{fighter2.name} wins the duel!")

# Example usage:
if __name__ == "__main__":
    # Create players
    player1 = Player("Player 1", 80)
    player2 = Player("Player 2", 90)
    player3 = Player("Player 3", 70)
    
    # Create an Arena instance
    arena = Arena("Arena 1")
    
    # Add combatants to the arena
    arena.addCombatant(player1)
    arena.addCombatant(player2)
    arena.addCombatant(player3)  # This combatant won't be added due to duplicate
    
    # List combatants in the arena
    arena.listCombatants()
    
    # Restore all combatants' health
    arena.restoreCombatants()
    
    # Perform a duel in the arena
    arena.duel()
class Combatant:
    def __init__(self, name, combat_class, max_health, attack, defense):
        self.name = name
        self.combat_class = combat_class
        self.max_health = max_health
        self.current_health = max_health
        self.attack = attack
        self.defense = defense
    
    def attack_enemy(self, enemy):
        damage = self.calculate_damage(enemy)
        enemy.take_damage(damage)
    
    def calculate_damage(self, enemy):
        # Base damage calculation (can be overridden by subclasses)
        return self.attack - enemy.defense
    
    def take_damage(self, damage):
        if damage > 0:
            self.current_health -= damage
            if self.current_health <= 0:
                print(f"{self.name} has been KO'd!")
        else:
            print(f"No damage taken by {self.name}.")
    
    def reset_health(self):
        self.current_health = self.max_health
    
    def details(self):
        return f"Name: {self.name}, Class: {self.combat_class}, Stats: Health: {self.current_health}/{self.max_health}, Attack: {self.attack}, Defense: {self.defense}"

class Ranger(Combatant):
    def __init__(self, name, max_health=100, attack=20, defense=10):
        super().__init__(name, "Ranger", max_health, attack, defense)
    
    # Override calculate_damage method for Ranger class
    def calculate_damage(self, enemy):
        # Rangers have a special way of calculating damage, for example:
        # Custom logic specific to Ranger class
        return self.attack - enemy.defense + 5

# 示例用法：
if __name__ == "__main__":
    ranger = Ranger("Aragorn")
    warrior = Combatant("Gimli", "Warrior", 120, 25, 15)
    
    print(ranger.details())
    print(warrior.details())
    
    ranger.attack_enemy(warrior)
    print(warrior.details())  # 显示战士的剩余生命值等信息

    warrior.attack_enemy(ranger)
    print(ranger.details())  # 显示游侠的剩余生命值等信息

    ranger.reset_health()
    print(ranger.details())  # 重置游侠的生命值到最大值
class Mage:
    def __init__(self, name, magic_level, specialization):
        self.name = name
        self.magic_level = magic_level
        self.specialization = specialization
        self.mana = magic_level
        self.regenRate = magic_level / 4
    
    def cast_spell(self):
        # Base spell casting method (to be overridden by subclasses)
        raise NotImplementedError("Subclasses should implement cast_spell method.")
    
    def reset_values(self):
        self.mana = self.magic_level
    
    def details(self):
        return f"Name: {self.name}, Specialization: {self.specialization}, Magic Level: {self.magic_level}, Mana: {self.mana}, Regen Rate: {self.regenRate}"

class Pyromancer(Mage):
    def __init__(self, name, magic_level):
        super().__init__(name, magic_level, "Pyro")
    
    def cast_spell(self):
        # Custom spell casting logic for Pyromancer
        spell_power = self.mana * 2
        print(f"{self.name} casts a fiery spell with power {spell_power}!")
        return spell_power

class FrostMage(Mage):
    def __init__(self, name, magic_level):
        super().__init__(name, magic_level, "Frost")
    
    def cast_spell(self):
        # Custom spell casting logic for FrostMage
        spell_power = self.mana * 1.5
        print(f"{self.name} casts a frosty spell with power {spell_power}!")
        return spell_power

# 示例用法：
if __name__ == "__main__":
    pyromancer = Pyromancer("Ignis", 50)
    frost_mage = FrostMage("Aurelia", 40)
    
    print(pyromancer.details())
    print(frost_mage.details())
    
    pyromancer.cast_spell()
    frost_mage.cast_spell()
    
    pyromancer.reset_values()
    frost_mage.reset_values()
    
    print(pyromancer.details())
    print(frost_mage.details())
class PyroMage(Mage):
    def __init__(self, name, magic_level, strength_level):
        super().__init__(name, magic_level, "Pyro")
        self.strength_level = strength_level
        self.flameBoost = 1  # Initial flameBoost value
    
    def cast_spell(self):
        if self.mana >= 40:
            self.super_heat()
        elif self.mana > 10:
            self.fire_blast()
        else:
            print(f"{self.name} does not have enough mana to cast a spell!")
            return 0
    
    def super_heat(self):
        if self.mana >= 40:
            print(f"{self.name} casts SuperHeat!")
            self.flameBoost += 1
            self.mana -= 40
            self.mana += self.regenRate
            damage = (self.strength_level * self.flameBoost)
            print(f"Damage dealt: {damage}")
            return damage
        else:
            print(f"{self.name} does not have enough mana to cast SuperHeat!")
            return 0
    
    def fire_blast(self):
        if 10 < self.mana < 40:
            print(f"{self.name} casts FireBlast!")
            self.mana -= 10
            self.mana += self.regenRate
            damage = (self.strength_level * self.flameBoost) + 10
            print(f"Damage dealt: {damage}")
            return damage
        else:
            print(f"{self.name} does not have enough mana to cast FireBlast!")
            return 0

# 示例用法：
if __name__ == "__main__":
    pyro_mage = PyroMage("PyroMan", 50, 5)
    
    print(pyro_mage.details())
    
    pyro_mage.cast_spell()  # Casts SuperHeat
    pyro_mage.cast_spell()  # Casts FireBlast
    pyro_mage.cast_spell()  # Tries to cast, but not enough mana
    
    pyro_mage.reset_values()
    
    print(pyro_mage.details())
class Mage:
    def __init__(self, name, magic_level, mage_type):
        self.name = name
        self.magic_level = magic_level
        self.mage_type = mage_type
        self.mana = 100  # Starting mana
        self.regenRate = 10  # Mana regeneration rate
    
    def details(self):
        return f"{self.name} ({self.mage_type} Mage) | Magic Level: {self.magic_level} | Mana: {self.mana}"
    
    def reset_values(self):
        self.mana = 100
    
    def cast_spell(self):
        pass
    
    def receive_damage(self, damage):
        pass


class FrostMage(Mage):
    def __init__(self, name, magic_level):
        super().__init__(name, magic_level, "Frost")
        self.iceBlock = False
    
    def cast_spell(self):
        if self.mana >= 50:
            self.setup_ice_block()
        elif 10 < self.mana < 50:
            self.ice_barrage()
        else:
            print(f"{self.name} does not have enough mana to cast a spell!")
            return 0
    
    def setup_ice_block(self):
        if self.mana >= 50:
            print(f"{self.name} sets up Ice Block!")
            self.iceBlock = True
            self.mana -= 50
            self.mana += self.regenRate
            return True
        else:
            print(f"{self.name} does not have enough mana to set up Ice Block!")
            return False
    
    def ice_barrage(self):
        if 10 < self.mana < 50:
            print(f"{self.name} casts Ice Barrage!")
            self.mana -= 10
            self.mana += self.regenRate
            damage = (self.magic_level / 4) + 30
            print(f"Damage dealt: {damage}")
            return damage
        else:
            print(f"{self.name} does not have enough mana to cast Ice Barrage!")
            return 0
    
    def receive_damage(self, damage):
        if self.iceBlock:
            print(f"Ice Block absorbs the damage from {self.name}'s attack!")
            self.iceBlock = False
        else:
            print(f"{self.name} receives {damage} damage!")
            self.mana += self.regenRate
if __name__ == "__main__":
    frost_mage = FrostMage("Frosty", 40)
    
    print(frost_mage.details())
    
    frost_mage.cast_spell()  # Tries to setup Ice Block, not enough mana
    frost_mage.cast_spell()  # Casts Ice Barrage
    
    frost_mage.receive_damage(25)  # Ice Block should absorb this damage
    
    frost_mage.cast_spell()  # Sets up Ice Block
    frost_mage.receive_damage(40)  # Ice Block absorbs this damage
    
    print(frost_mage.details())
    
    frost_mage.reset_values()
    
    print(frost_mage.details())

class Ranger:
    def __init__(self, name, ranged_level, strength_level):
        self.name = name
        self.ranged_level = ranged_level
        self.strength_level = strength_level
        self.arrows = 3
    
    def details(self):
        return f"{self.name} | Ranged Level: {self.ranged_level} | Strength Level: {self.strength_level} | Arrows: {self.arrows}"
    
    def reset_values(self):
        self.arrows = 3
    
    def fire_arrow(self):
        if self.arrows > 0:
            self.arrows -= 1
            if self.arrows == 0:
                print(f"{self.name} fires the last arrow!")
            damage = self.ranged_level
            print(f"{self.name} fires an arrow! Damage dealt: {damage}")
            return damage
        else:
            print(f"{self.name} is out of arrows!")
            return 0
    
    def melee_attack(self):
        damage = self.strength_level
        print(f"{self.name} performs a melee attack! Damage dealt: {damage}")
        return damage


# Example usage:
if __name__ == "__main__":
    ranger = Ranger("Artemis", 30, 25)
    
    print(ranger.details())
    
    ranger.fire_arrow()
    ranger.melee_attack()
    ranger.fire_arrow()
    ranger.fire_arrow()
    ranger.fire_arrow()
    
    print(ranger.details())
    
    ranger.reset_values()
    
    print(ranger.details())
class Warrior:
    def __init__(self, name, strength_level, defence_level):
        self.name = name
        self.strength_level = strength_level
        self.defence_level = defence_level
        self.armour_value = 10
    
    def details(self):
        return f"{self.name} | Strength Level: {self.strength_level} | Defence Level: {self.defence_level} | Armour Value: {self.armour_value}"
    
    def reset_values(self):
        self.armour_value = 10
    
    def take_damage(self, damage):
        total_defence = self.defence_level + self.armour_value
        actual_damage = max(damage - total_defence, 0)
        
        if actual_damage > 0:
            self.armour_value -= 5
            if self.armour_value <= 0:
                print(f"{self.name}'s armour is shattered!")
                self.armour_value = 0
            print(f"{self.name} takes {actual_damage} damage!")
        else:
            print(f"{self.name} takes no damage!")
        
    def calculate_power(self):
        return self.strength_level
    
# Example usage:
if __name__ == "__main__":
    warrior = Warrior("Conan", 35, 20)
    
    print(warrior.details())
    
    warrior.take_damage(15)
    warrior.take_damage(30)
    warrior.take_damage(25)
    
    print(warrior.details())
    
    warrior.reset_values()
    
    print(warrior.details())
class BlessedWarrior:
    def __init__(self, name, max_health, base_damage):
        self.name = name
        self.max_health = max_health
        self.current_health = max_health
        self.base_damage = base_damage
    
    def details(self):
        return f"{self.name} | Health: {self.current_health}/{self.max_health} | Base Damage: {self.base_damage}"
    
    def take_damage(self, damage):
        self.current_health -= damage
        if self.current_health < 0:
            self.current_health = 0
        print(f"{self.name} takes {damage} damage! Current Health: {self.current_health}/{self.max_health}")
    
    def heal(self, amount):
        self.current_health += amount
        if self.current_health > self.max_health:
            self.current_health = self.max_health
        print(f"{self.name} heals {amount} HP. Current Health: {self.current_health}/{self.max_health}")
    
    def calculate_damage(self):
        missing_health = self.max_health - self.current_health
        bonus_damage = missing_health
        
        # Ensure damage doesn't exceed base damage
        actual_damage = min(self.base_damage + bonus_damage, self.base_damage * 2)
        
        return actual_damage
    
# Example usage:
if __name__ == "__main__":
    dharok = BlessedWarrior("Dharok", 100, 20)
    
    print(dharok.details())
    
    # Simulate taking damage and healing
    dharok.take_damage(30)
    dharok.take_damage(15)
    dharok.heal(10)
    
    # Calculate damage with current health
    calculated_damage = dharok.calculate_damage()
    print(f"{dharok.name} attacks with {calculated_damage} damage!")
    
    # Simulate more damage taken
    dharok.take_damage(50)
    
    # Recalculate damage after more damage taken
    calculated_damage = dharok.calculate_damage()
    print(f"{dharok.name} attacks with {calculated_damage} damage!")
class BlessedWarrior:
    def __init__(self, name, base_damage, range_level):
        self.name = name
        self.base_damage = base_damage
        self.range_level = range_level
    
    def details(self):
        return f"{self.name} | Base Damage: {self.base_damage} | Range Level: {self.range_level}"
    
    def increase_range_level(self, level):
        self.range_level += level
        print(f"{self.name} increases range level by {level}! New Range Level: {self.range_level}")
    
    def calculate_damage(self):
        total_damage = self.base_damage + self.range_level
        return total_damage
    
# Example usage:
if __name__ == "__main__":
    karil = BlessedWarrior("Karil", 20, 5)
    
    print(karil.details())
    
    # Increase range level
    karil.increase_range_level(3)
    
    # Calculate damage with current stats
    calculated_damage = karil.calculate_damage()
    print(f"{karil.name} attacks with {calculated_damage} damage!")

