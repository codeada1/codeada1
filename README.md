import pygame
import random

# Ekran boyutları
WIDTH = 800
HEIGHT = 600

# Renkler
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Tank sınıfı
class Tank(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 30))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH // 2, HEIGHT // 2)
        self.speed = 5

    def update(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]:
            self.rect.x -= self.speed
        if keys[pygame.K_RIGHT]:
            self.rect.x += self.speed
        if keys[pygame.K_UP]:
            self.rect.y -= self.speed
        if keys[pygame.K_DOWN]:
            self.rect.y += self.speed

# Düşman sınıfı
class Enemy(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((30, 30))
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, WIDTH - self.rect.width)
        self.rect.y = random.randint(0, HEIGHT - self.rect.height)

# Oyun başlatma
pygame.init()
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Tank Oyunu")
clock = pygame.time.Clock()

# Sprite grupları
all_sprites = pygame.sprite.Group()
enemies = pygame.sprite.Group()

# Tank oluşturma
tank = Tank()
all_sprites.add(tank)

# Düşmanlar oluşturma
for i in range(5):
    enemy = Enemy()
    all_sprites.add(enemy)
    enemies.add(enemy)

# Oyun döngüsü
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Oyun güncelleme
    all_sprites.update()

    # Tank ve düşmanların çarpışma kontrolü
    hits = pygame.sprite.spritecollide(tank, enemies, True)
    if hits:
        running = False

    # Ekranı temizleme
    screen.fill(BLACK)

    # Tüm sprite'ları çizme
    all_sprites.draw(screen)

    # Ekranı güncelleme
    pygame.display.flip()

    # Oyun saatini ayarlama
    clock.tick(30)

pygame.quit()
