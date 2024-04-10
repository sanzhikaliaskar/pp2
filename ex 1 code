# Randomly generating coins with different weights on the road
# Increase the speed of Enemy when the player earns N coins
# Comment your code

#Imports
import pygame, sys
from pygame.locals import *
import random, time

#Initialzing 
pygame.init()

#Setting up FPS 
FPS = 60
FramePerSec = pygame.time.Clock()

#Creating colors
BLUE  = (0, 0, 255)
RED   = (255, 0, 0)
GREEN = (0, 255, 0)
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)

#Other Variables for use in the program
SCREEN_WIDTH = 400
SCREEN_HEIGHT = 600
SPEED = 5
SCORE = 0
MONEY_SCORE = 0
earned_coins = 0

#Setting up Fonts
font = pygame.font.SysFont("Verdana", 60)
font_small = pygame.font.SysFont("Verdana", 20)
game_over = font.render("Game Over", True, BLACK)

background = pygame.image.load("lab9//ex_1//sprites//AnimatedStreet.png")

#Create a white screen 
DISPLAYSURF = pygame.display.set_mode((400,600))
DISPLAYSURF.fill(WHITE)
pygame.display.set_caption("Game")


class Enemy(pygame.sprite.Sprite):
      def __init__(self):
        super().__init__() 
        self.image = pygame.image.load("lab9//ex_1//sprites//Enemy.png")
        self.rect = self.image.get_rect()
        self.rect.center = (random.randint(40,SCREEN_WIDTH-40), 0)

      def move(self):
        global SCORE
        self.rect.move_ip(0,SPEED)
        if (self.rect.bottom > 600):
            SCORE += 1
            self.rect.top = 0
            self.rect.center = (random.randint(40, SCREEN_WIDTH - 40), 0)


class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__() 
        self.image = pygame.image.load("lab9//ex_1//sprites//Player.png")
        self.rect = self.image.get_rect()
        self.rect.center = (160, 520)
       
    def move(self):
        pressed_keys = pygame.key.get_pressed()
        
        if self.rect.left > 0:
              if pressed_keys[K_LEFT]:
                  self.rect.move_ip(-5, 0)
        if self.rect.right < SCREEN_WIDTH:        
              if pressed_keys[K_RIGHT]:
                  self.rect.move_ip(5, 0)


# Yeah that's a comment
class Coin(pygame.sprite.Sprite):
    def __init__(self, coin_image, weight):
            super().__init__() 
            self.image = pygame.image.load(coin_image)
            self.image = pygame.transform.scale(self.image, (50, 50))

            self.weight = weight

            self.rect = self.image.get_rect()
            self.rect.center = (random.randint(50,SCREEN_WIDTH-50), 0)

            self.flag = False
    
    def move(self):
        self.rect.move_ip(0,SPEED)
        self.flag = 0
        if (self.rect.bottom > 600):
            self.flag = True
                  

#Setting up Sprites        
P1 = Player()
E1 = Enemy()
C1 = Coin("lab9//ex_1//sprites//Coin.png", 1) # Comment
C2 = Coin("lab9//ex_1//sprites//Coin2.png", 5)


#Creating Sprites Groups
enemies = pygame.sprite.Group()
enemies.add(E1)
# Some Comments
money = pygame.sprite.Group()
money.add(C1)
money.add(C2)

money_to_move = 0

all_sprites = pygame.sprite.Group()
all_sprites.add(P1)
all_sprites.add(E1)

#Adding a new User event 
INC_SPEED = pygame.USEREVENT + 1
pygame.time.set_timer(INC_SPEED, 1000)

#Game Loop
while True:
      
    #Cycles through all events occuring  
    for event in pygame.event.get():
        if event.type == INC_SPEED:
              SPEED += 0.5      
        if event.type == QUIT:
            pygame.quit()
            sys.exit()
    
    if earned_coins - 3 == 0:
        earned_coins = 0
        SPEED += 0.5


    DISPLAYSURF.blit(background, (0,0))
    scores = font_small.render(str(SCORE), True, BLACK)
    DISPLAYSURF.blit(scores, (10,10))
    # Commented
    money_score = font_small.render(str(MONEY_SCORE), True, BLACK)
    DISPLAYSURF.blit(money_score, (SCREEN_WIDTH - 50, 10))

    #Moves and Re-draws all Sprites
    for entity in all_sprites:
        entity.move()
        DISPLAYSURF.blit(entity.image, entity.rect)
    
    i = 0
    for entity in money:
        if i == money_to_move:
            entity.move()
            DISPLAYSURF.blit(entity.image, entity.rect)

            if entity.flag:
                entity.rect.center = (random.randint(-50, SCREEN_WIDTH - 40), 0)
                money_to_move = random.randrange(0, 2) # 2 is len of my coins
        i += 1
                

        

    #To be run if collision occurs between Player and Enemy
    if pygame.sprite.spritecollideany(P1, enemies):
        pygame.mixer.Sound('lab9//ex_1//songs//crash.wav').play()
        time.sleep(1)
                
        DISPLAYSURF.fill(RED)
        DISPLAYSURF.blit(game_over, (30,250))
        
        pygame.display.update()
        for entity in all_sprites:
            entity.kill() 
        
        for entity in money:
            entity.kill()
        time.sleep(2)
        pygame.quit()
        sys.exit()    

    # Commented
    
    for entity in money:
        if pygame.Rect.colliderect(entity.rect, P1.rect):
            entity.rect.center = (random.randint(-50, SCREEN_WIDTH - 40), 0)
            money_to_move = random.randrange(0, 2) # 2 is len of my coins
            MONEY_SCORE += entity.weight
            earned_coins += 1
        
        
    pygame.display.update()
    FramePerSec.tick(FPS)
