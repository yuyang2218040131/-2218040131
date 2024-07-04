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
            
