# snake-game
import pygame
import random
import os

pygame.mixer.init()

pygame.init()

#GAme intro and outro
introa = pygame.image.load("full_intro.png")
outroa = pygame.image.load("Outroa_for_snakegame.png")



# Colors
wood=		(255,255,255)
yellow=	(255,255,0)
white = (124,252,0)
red = (139,0,0)
black = (0,0,128)
green=(124,222,120)
# Creating window
screen_width = 900
screen_height = 600
gameWindow = pygame.display.set_mode((screen_width, screen_height))

#Background Image
bgimg = pygame.image.load("pexels-sebastiaan-stam-1097456.jpg")
bgimg = pygame.transform.scale(bgimg, (screen_width, screen_height)).convert_alpha()

# snake_image_for_game=pygame.image.load()
# intro = pygame.image.load("Screen/intro1.png")
# outro = pygame.image.load("Screen/outro.png")

# Game Title
pygame.display.set_caption("SnakesWithRohit")
pygame.display.update()
clock = pygame.time.Clock()
font = pygame.font.SysFont(None, 55)


def text_screen(text, color, x, y):
    screen_text = font.render(text, True, color)
    gameWindow.blit(screen_text, [x,y])


def plot_snake(gameWindow, color, snk_list, snake_size):
    for x,y in snk_list:
        pygame.draw.rect(gameWindow, color, [x, y, snake_size, snake_size])



def welcome():
    exit_game = False
    while not exit_game:
        gameWindow.blit(introa, (0,0))
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                exit_game = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_RETURN:
                    pygame.mixer.music.fadeout(200)
                    pygame.mixer.music.load('snake_music.mp3')
                    pygame.mixer.music.play(100)
                    pygame.mixer.music.set_volume(.6)
                    gameloop()
        pygame.display.update()
        clock.tick(60)

# def welcome():
#     exit_game = False
#     while not exit_game:
#         while not exit_game:
#             gameWindow.blit(introa, (0, 0))
#             for event in pygame.event.get():
#                 if event.type == pygame.QUIT:
#                     exit_game = True
#                     if event.type == pygame.KEYDOWN:
#                         if event.key == pygame.K_SPACE:
#                             pygame.mixer.music.load('snake_music.mp3')
#                             pygame.mixer.music.play(-1)
#                             pygame.display.update()
#                             clock.tick(60)
#
#                             gameloop()

# Game Loop
def gameloop():
    # Game specific variables
    exit_game = False
    game_over = False
    snake_x = 45
    snake_y = 55
    velocity_x = 0
    velocity_y = 0
    snk_list = []
    snk_length = 1
    # Check if hiscore file exists
    if(not os.path.exists("hiscore.txt")):
        with open("hiscore.txt", "w") as f:
            f.write("0")

    with open("hiscore.txt", "r") as f:
        hiscore = f.read()

    food_x = random.randint(30, screen_width / 2)
    food_y = random.randint(30, screen_height / 2)
    score = 0
    init_velocity = 5
    snake_size = 30
    fps = 40
    while not exit_game:
        if game_over:
            with open("hiscore.txt", "w") as f:
                f.write(str(hiscore))
            gameWindow.fill(white)
            gameWindow.blit(outroa, (0, 0))
            text_screen("Score: " + str(score) ,black,333,220)
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    exit_game = True

                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_RETURN:
                        welcome()

        else:

            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    exit_game = True

                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_RIGHT:
                        velocity_x = init_velocity
                        velocity_y = 0

                    if event.key == pygame.K_LEFT:
                        velocity_x = - init_velocity
                        velocity_y = 0

                    if event.key == pygame.K_UP:
                        velocity_y = - init_velocity
                        velocity_x = 0

                    if event.key == pygame.K_DOWN:
                        velocity_y = init_velocity
                        velocity_x = 0

                    if event.key == pygame.K_q:
                        score +=10

            snake_x = snake_x + velocity_x
            snake_y = snake_y + velocity_y

            if abs(snake_x - food_x)<15 and abs(snake_y - food_y)<15:
                score +=10
                food_x = random.randint(20, screen_width / 2)
                food_y = random.randint(20, screen_height / 2)
                snk_length +=5
                if score>int(hiscore):
                    hiscore = score

            gameWindow.fill(white)
            gameWindow.blit(bgimg, (0, 0))
            text_screen("Score: " + str(score) + "  Hiscore: "+str(hiscore), yellow, 5, 5)
            pygame.draw.rect(gameWindow,wood, [food_x, food_y, snake_size, snake_size])
            text_screen("Enjoy game",red,680,560)


            head = []
            head.append(snake_x)
            head.append(snake_y)
            snk_list.append(head)

            if len(snk_list)>snk_length:
                del snk_list[0]

            if head in snk_list[:-1]:
                game_over = True
                pygame.mixer.music.load('game_over_sound.mp3')
                pygame.mixer.music.play()

            if snake_x<0 or snake_x>screen_width or snake_y<0 or snake_y>screen_height:
                game_over = True
                pygame.mixer.music.load('game_over_sound.mp3')
                pygame.mixer.music.play()
            plot_snake(gameWindow, black, snk_list, snake_size)
        pygame.display.update()
        clock.tick(fps)

    pygame.quit()
    quit()
welcome()
![full_intro](https://user-images.githubusercontent.com/106584386/183259598-e0b0dced-f8d2-4ead-b548-45a0a1d269bb.png)
![Outroa_for_snakegame](https://user-images.githubusercontent.com/106584386/183259604-60a4de0d-4c45-4713-b6f1-74f0b5f3e715.png)
![pexels-sebastiaan-stam-1097456](https://user-images.githubusercontent.com/106584386/183259607-b63aed87-f543-4c6f-8b8c-e7fab5dbc708.jpg)
