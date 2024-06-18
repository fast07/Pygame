import pygame
import random

# Initialize Pygame
pygame.init()

# Screen dimensions
SCREEN_WIDTH = 600
SCREEN_HEIGHT = 400

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Create the screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('Simple Game')

# Player settings
player_width = 50
player_height = 50
player_x = SCREEN_WIDTH // 2 - player_width // 2
player_y = SCREEN_HEIGHT - player_height - 20
player_speed = 5

# Obstacle settings
obstacle_width = 50
obstacle_height = 50
obstacle_speed = 3

# List to hold all obstacles
obstacles = []

# Game loop
running = True
clock = pygame.time.Clock()

while running:
    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Move the player
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_x -= player_speed
    if keys[pygame.K_RIGHT]:
        player_x += player_speed

    # Ensure player stays within screen boundaries
    if player_x < 0:
        player_x = 0
    elif player_x > SCREEN_WIDTH - player_width:
        player_x = SCREEN_WIDTH - player_width

    # Add new obstacle at the top of the screen
    if random.randint(1, 100) == 1:
        obstacle_x = random.randint(0, SCREEN_WIDTH - obstacle_width)
        obstacle_y = -obstacle_height
        obstacles.append([obstacle_x, obstacle_y])

    # Move obstacles down the screen
    for obstacle in obstacles:
        obstacle[1] += obstacle_speed

    # Remove obstacles that have fallen off the screen
    obstacles = [obstacle for obstacle in obstacles if obstacle[1] < SCREEN_HEIGHT]

    # Check for collisions between player and obstacles
    player_rect = pygame.Rect(player_x, player_y, player_width, player_height)
    for obstacle in obstacles:
        obstacle_rect = pygame.Rect(obstacle[0], obstacle[1], obstacle_width, obstacle_height)
        if player_rect.colliderect(obstacle_rect):
            running = False

    # Clear the screen
    screen.fill(WHITE)

    # Draw the player
    pygame.draw.rect(screen, BLACK, (player_x, player_y, player_width, player_height))

    # Draw obstacles
    for obstacle in obstacles:
        pygame.draw.rect(screen, BLACK, (obstacle[0], obstacle[1], obstacle_width, obstacle_height))

    # Update the screen
    pygame.display.flip()

    # Cap the frame rate
    clock.tick(60)

# Quit Pygame
pygame.quit()
