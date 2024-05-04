import pygame
import random
import math
from pygame import mixer

pygame.init()
clock = pygame.time.Clock()

screen = pygame.display.set_mode((800, 600))
background = pygame.image.load('AdobeStock_128985367_Preview.jpeg')
mixer.music.load('background.wav')
mixer.music.play(-1)
pygame.display.set_caption("Galactic Vanguard")
icon = pygame.image.load('spaceship.png')
pygame.display.set_icon(icon)

playerImage = pygame.image.load('Player.png')
playerX = 370
playerY = 480
playerChange = 0

enemyImage = []
enemyXAxis = []
enemyYAxis = []
enemyXChange = []
enemyYChange = []
multiple_enemies = 6
for i in range(multiple_enemies):
    enemyImage.append(pygame.image.load('alien.png'))
    enemyXAxis.append(random.randint(0, 735))
    enemyYAxis.append(random.randint(50, 150))
    enemyXChange.append(0.3)
    enemyYChange.append(40)

BulletImage = pygame.image.load('bullet.png')
BulletX = 0
BulletY = 480
BulletXChange = 0
BulletYChange = 5
State = "Ready"

#Score
scoreValue = 0
font = pygame.font.Font('freesansbold.ttf',32)
overFont = pygame.font.Font('freesansbold.ttf',64)

textX = 10
textY = 10

game_over = False

def gameOver():
    itsOver = overFont.render("GAME OVER",  True, (255, 255, 255))
    screen.blit(itsOver, (200, 250))

def show_score(x,y):
    theScore = font.render("Score: " + str(scoreValue),True,(255,255,255))
    screen.blit(theScore, (x, y))

def Player(x, y):
    screen.blit(playerImage, (x, y))

def enemy(x, y,i):
    screen.blit(enemyImage[i], (x, y))

def bullet(x, y):
    screen.blit(BulletImage, (x + 16, y + 10))

def Collision(enemyX, enemyY, BulletX, BulletY):
    Distance = math.sqrt(math.pow(enemyX - BulletX, 2) + math.pow(enemyY - BulletY, 2))
    if Distance < 27:
        return True
    else:
        return False

running = True
bullet_speed = 1

while running:
    screen.fill((0, 0, 0))
    screen.blit(background, (0, 0))
    playerSpeed = 0.3

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                playerChange = -playerSpeed
            elif event.key == pygame.K_RIGHT:
                playerChange = playerSpeed
            elif event.key == pygame.K_SPACE:
                if State == "Ready":
                    BSound = mixer.Sound('laser.wav')
                    BSound.play()
                    BulletX = playerX
                    State = "Fire"

        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                playerChange = 0

    playerX += playerChange

    if playerX <= 0:
        playerX = 0
    elif playerX > 736:
        playerX = 736

    for i in range(multiple_enemies):
        if enemyYAxis[i] >= 440:
            game_over = True
            break

        enemyXAxis[i] += enemyXChange[i]
        if enemyXAxis[i] <= 0:
            enemyXChange[i] = 0.3
            enemyYAxis[i] += enemyYChange[i]
        elif enemyXAxis[i] > 736:
            enemyXChange[i] = -0.3
            enemyYAxis[i] += enemyYChange[i]

        enemyXAxis[i] += enemyXChange[i]
        theCollision = Collision(enemyXAxis[i], enemyYAxis[i], BulletX, BulletY)
        if theCollision:
            ESound = mixer.Sound('explosion.wav')
            ESound.play()
            BulletY = 480
            State = "Ready"
            scoreValue += 1
            enemyXAxis[i] = random.randint(0, 735)
            enemyYAxis[i] = random.randint(50, 150)

        if enemyYAxis[i] >= 480:
            enemyXAxis[i] = random.randint(0, 735)
            enemyYAxis[i] = random.randint(50, 150)

        enemy(enemyXAxis[i], enemyYAxis[i], i)

    if BulletY <= 0:
        BulletY = 480
        State = "Ready"

    if State == "Fire":
        bullet(BulletX, BulletY)
        BulletY -= bullet_speed
        if BulletY <= 0:
            State = "Ready"

    Player(playerX, playerY)
    show_score(textX,textY)

    if game_over:
        gameOver()

    pygame.display.update()

    if game_over:
        pygame.time.wait(3000)  # Wait for 3 seconds before exiting
        break
