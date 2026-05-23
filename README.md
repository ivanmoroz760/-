import pygame
import random
import sys

# Инициализация Pygame
pygame.init()

# Настройки экрана
WIDTH, HEIGHT = 640, 480
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Змейка")

# Цвета
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Настройки змейки
snake_size = 20
snake_speed = 10

# Начальное положение змейки
snake = [(WIDTH // 2, HEIGHT // 2)]
direction = (snake_size, 0)  # Начинаем движение вправо

# Еда
food = (random.randint(0, WIDTH - snake_size) // snake_size * snake_size,
          random.randint(0, HEIGHT - snake_size) // snake_size * snake_size)

# Часы для контроля FPS
clock = pygame.time.Clock()

# Основной игровой цикл
running = True
while running:
    # Обработка событий
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP and direction != (0, snake_size):
                direction = (0, -snake_size)
            elif event.key == pygame.K_DOWN and direction != (0, -snake_size):
                direction = (0, snake_size)
            elif event.key == pygame.K_LEFT and direction != (snake_size, 0):
                direction = (-snake_size, 0)
            elif event.key == pygame.K_RIGHT and direction != (-snake_size, 0):
                direction = (snake_size, 0)

    # Движение змейки
    new_head = (snake[0][0] + direction[0], snake[0][1] + direction[1])
    snake.insert(0, new_head)

    # Проверка на съедание еды
    if snake[0] == food:
        # Создаём новую еду
        food = (random.randint(0, WIDTH - snake_size) // snake_size * snake_size,
              random.randint(0, HEIGHT - snake_size) // snake_size * snake_size)
    else:
        snake.pop()  # Удаляем хвост, если еда не съедена

    # Проверка на столкновение со стеной или собой
    if (snake[0][0] < 0 or snake[0][0] >= WIDTH or
        snake[0][1] < 0 or snake[0][1] >= HEIGHT or
        snake[0] in snake[1:]):
        running = False  # Игра окончена

    # Отрисовка
    screen.fill(BLACK)
    for segment in snake:
        pygame.draw.rect(screen, GREEN, (segment[0], segment[1], snake_size, snake_size))
    pygame.draw.rect(screen, RED, (food[0], food[1], snake_size, snake_size))

    pygame.display.flip()
    clock.tick(snake_speed)

# Завершение игры
pygame.quit()
sys.exit()
