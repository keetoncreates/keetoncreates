
Download
Copy code
import pygame
import random

# Initialize Pygame
pygame.init()

# Set up some constants
WIDTH = 800
HEIGHT = 600
BALL_RADIUS = 10
PADDLE_WIDTH = 10
PADDLE_HEIGHT = 60
HALF_PADDLE_WIDTH = PADDLE_WIDTH / 2
HALF_PADDLE_HEIGHT = PADDLE_HEIGHT / 2

# Set up the display
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption('Ping Pong')

# Set up the colors
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)

# Set up the paddles
left_paddle = pygame.Rect(HALF_PADDLE_WIDTH, HEIGHT / 2 - HALF_PADDLE_HEIGHT, PADDLE_WIDTH, PADDLE_HEIGHT)
right_paddle = pygame.Rect(WIDTH - HALF_PADDLE_WIDTH, HEIGHT / 2 - HALF_PADDLE_HEIGHT, PADDLE_WIDTH, PADDLE_HEIGHT)

# Set up the ball
ball = pygame.Rect(WIDTH / 2 - BALL_RADIUS, HEIGHT / 2 - BALL_RADIUS, BALL_RADIUS * 2, BALL_RADIUS * 2)
ball_speed_x = random.choice([-2, 2])
ball_speed_y = random.choice([-2, 2])

# Set up the game clock
clock = pygame.time.Clock()

# Main game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_w:
                left_paddle.y -= 5
            elif event.key == pygame.K_s:
                left_paddle.y += 5
            elif event.key == pygame.K_UP:
                right_paddle.y -= 5
            elif event.key == pygame.K_DOWN:
                right_paddle.y += 5

    # Keep the paddles on the screen
    if left_paddle.top < 0:
        left_paddle.top = 0
    if left_paddle.bottom > HEIGHT:
        left_paddle.bottom = HEIGHT
    if right_paddle.top < 0:
        right_paddle.top = 0
    if right_paddle.bottom > HEIGHT:
        right_paddle.bottom = HEIGHT

    # Move the ball
    ball.x += ball_speed_x
    ball.y += ball_speed_y

    # Check for collisions with the top and bottom of the screen
    if ball.top <= 0 or ball.bottom >= HEIGHT:
        ball_speed_y = -ball_speed_y

    # Check for collisions with the paddles
    if ball.colliderect(left_paddle) or ball.colliderect(right_paddle):
        ball_speed_x = -ball_speed_x

    # Check for a score
    if ball.left <= 0:
        ball.left = WIDTH / 2 - BALL_RADIUS
        ball_speed_x = random.choice([-2, 2])
    elif ball.right >= WIDTH:
        ball.right = WIDTH / 2 - BALL_RADIUS
        ball_speed_x = -random.choice([-2, 2])

    # Draw everything
    screen.fill(WHITE)
    pygame.draw.ellipse(screen, GREEN, ball)
    pygame.draw.rect(screen, GREEN, left_paddle)
    pygame.draw.rect(screen, GREEN, right_paddle)

    # Update the display
    pygame.display.flip()
