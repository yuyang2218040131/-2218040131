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

            

        
