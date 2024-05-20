import pygame
import sys

# מימדי המסך
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# צבעים
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# מחלקת השחקן
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.center = (50, SCREEN_HEIGHT // 2)
        self.velocity_y = 0
        self.gravity = 0.8

    def update(self):
        self.velocity_y += self.gravity
        self.rect.y += self.velocity_y

        # פיקודי שחקן
        keys = pygame.key.get_pressed()
        if keys[pygame.K_SPACE]:  # קפיצה
            self.velocity_y = -15

        # גבולות המסך
        if self.rect.top <= 0:
            self.rect.top = 0
            self.velocity_y = 0
        elif self.rect.bottom >= SCREEN_HEIGHT:
            self.rect.bottom = SCREEN_HEIGHT
            self.velocity_y = 0

# אתחול
pygame.init()
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("פרקור קשה")
clock = pygame.time.Clock()

all_sprites = pygame.sprite.Group()
player = Player()
all_sprites.add(player)

# לולאת המשחק
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # עדכון
    all_sprites.update()

    # ציור
    screen.fill(WHITE)
    all_sprites.draw(screen)

    # הצגה
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
sys.exit()
