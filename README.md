"""
Created 12/8/23

@author: Simon Price
"""

import pygame
import random
import sys

pygame.init()

#parameters
WIDTH, HEIGHT = 800, 600
FPS = 60
CAR_WIDTH, CAR_HEIGHT = 50, 80
OBSTACLE_WIDTH, OBSTACLE_HEIGHT = 50, 50
CAR_SPEED = 5
OBSTACLE_SPEED = 5

WHITE = (255, 255, 255)
RED = (255, 0, 0)

screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Car Dodge")

#images
car_img = pygame.image.load("car.png")  
obstacle_img = pygame.image.load("obstacle.png")  
background_img = pygame.image.load("background.png")  

car_rect = car_img.get_rect(center=(WIDTH // 2, HEIGHT - 50))
obstacles = []

clock = pygame.time.Clock()

#variables
score = 0
font = pygame.font.Font(None, 36)

#loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and car_rect.left > 0:
        car_rect.x -= CAR_SPEED
    if keys[pygame.K_RIGHT] and car_rect.right < WIDTH:
        car_rect.x += CAR_SPEED

    # Move obstacles and generate new ones
    for obstacle in obstacles:
        obstacle.y += OBSTACLE_SPEED
        if obstacle.colliderect(car_rect):
            pygame.quit()
            sys.exit()
        if obstacle.y > HEIGHT:
            obstacles.remove(obstacle)
            score += 1

    if random.randint(0, 100) < 5:
        obstacle_rect = obstacle_img.get_rect(center=(random.randint(0, WIDTH - OBSTACLE_WIDTH), 0))
        obstacles.append(obstacle_rect)

    screen.blit(background_img, (0, 0))
    screen.blit(car_img, car_rect)
    for obstacle in obstacles:
        screen.blit(obstacle_img, obstacle)

    score_text = font.render(f"Score: {score}", True, WHITE)
    screen.blit(score_text, (10, 10))

    pygame.display.flip()


    clock.tick(FPS)
