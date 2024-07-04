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
            

        
