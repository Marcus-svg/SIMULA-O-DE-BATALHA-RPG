import random
import time
from abc import ABC, abstractmethod


class RPGCharacter(ABC):
    
    def __init__(self, name, health, attack, defense):
        self.name = name
        self.max_health = health
        self.health = health
        self.attack = attack
        self.defense = defense
    
    @abstractmethod
    def describe_attack(self):
    
        pass
        
    def take_damage(self, amount):
      
        damage_taken = amount - self.defense
        if damage_taken > 0:
            self.health -= damage_taken
            print(f"-> {self.name} sofreu {damage_taken} de dano!")
            if not self.is_alive():
                print(f"-> {self.name} foi derrotado!")
        else:
            print(f"-> A defesa de {self.name} bloqueou o ataque!")

    def is_alive(self):
       
        return self.health > 0

    def __str__(self):
       
        return f"[{self.name} ({self.__class__.__name__}) | HP: {self.health}/{self.max_health}]"


class Warrior(RPGCharacter):
    def __init__(self, name):
        super().__init__(name=name, health=150, attack=15, defense=15)
    def describe_attack(self):
        return f"{self.name} avança com sua espada!"

class Archer(RPGCharacter):
    def __init__(self, name):
        super().__init__(name=name, health=100, attack=20, defense=5)
    def describe_attack(self):
        return f"{self.name} dispara uma flecha precisa!"

class Wizard(RPGCharacter):
    def __init__(self, name):
        super().__init__(name=name, health=80, attack=25, defense=3)
    def describe_attack(self):
        return f"{self.name} conjura uma bola de fogo!"

class Goblin(RPGCharacter):
    def __init__(self, name="Goblin"):
        super().__init__(name=name, health=40, attack=10, defense=8)
    def describe_attack(self):
        return f"O {self.name} ataca com sua cimitarra enferrujada!"

class Kobold(RPGCharacter):
    def __init__(self, name="Kobold"):
        super().__init__(name=name, health=50, attack=15, defense=4)
    def describe_attack(self):
        return f"O {self.name} arremessa uma pedra com sua funda!"
        
class Imp(RPGCharacter):
    def __init__(self, name="Imp"):
        super().__init__(name=name, health=40, attack=18, defense=6)
    def describe_attack(self):
        return f"O {self.name} ataca com sua cauda venenosa!"


def create_team(classes, size, prefix):
  
    team = []
    for i in range(size):
        char_class = random.choice(classes)
        team.append(char_class(f"{prefix} {i+1}"))
    return team

def get_living_characters(team):
    
    return [char for char in team if char.is_alive()]

def main():
   
    hero_classes = [Warrior, Archer, Wizard]
    villain_classes = [Goblin, Kobold, Imp]
    
    heroes = create_team(hero_classes, 3, "Herói")
    villains = create_team(villain_classes, 5, "Vilão")

    print("--- EQUIPES FORMADAS ---")
    print("Heróis:", ", ".join([str(h) for h in heroes]))
    print("Vilões:", ", ".join([str(v) for v in villains]))
    print("\n" + "="*20 + " A BATALHA COMEÇA! " + "="*20)
    
    turn = 1
    while len(get_living_characters(heroes)) > 0 and len(get_living_characters(villains)) > 0:
        print(f"\n--- TURNO {turn} ---")
        
       
        attacker_hero = random.choice(get_living_characters(heroes))
        target_villain = random.choice(get_living_characters(villains))
        
        print(attacker_hero.describe_attack())
        target_villain.take_damage(attacker_hero.attack)
        
        time.sleep(1)
        
        
        if len(get_living_characters(villains)) == 0:
            break
            
        
        attacker_villain = random.choice(get_living_characters(villains))
        target_hero = random.choice(get_living_characters(heroes))
        
        print(attacker_villain.describe_attack())
        target_hero.take_damage(attacker_villain.attack)
        
        time.sleep(2)
        turn += 1

    print("\n" + "="*22 + " FIM DE COMBATE " + "="*22)
    
    
    if len(get_living_characters(heroes)) > 0:
        print("\n OS HERÓIS VENCERAM")
        print("Sobreviventes:")
        for hero in get_living_characters(heroes):
            print(hero)
    else:
        print("\n OS VILÕES VENCERAM")
        print("Sobreviventes:")
        for villain in get_living_characters(villains):
            print(villain)


if __name__ == "__main__":
    main()
