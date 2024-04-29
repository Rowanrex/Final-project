





























import pygame
import random
import sys
import math


pygame.init()


WIDTH, HEIGHT = 600, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Nine Lives")


PINK = (255, 192, 203)  
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)


cat_img = pygame.image.load("cat.png")
dog_img = pygame.image.load("dog.png")
catnip_img = pygame.image.load("catnip.png")
grass_img = pygame.image.load("grass.png")
sun_img = pygame.image.load("sun.png")
cloud_img = pygame.image.load("cloud.png")


cat_img = pygame.transform.scale(cat_img, (100, 100))
dog_img = pygame.transform.scale(dog_img, (75, 75))
catnip_img = pygame.transform.scale(catnip_img, (50, 50))
grass_img = pygame.transform.scale(grass_img, (600, 100))
cloud_img = pygame.transform.scale(cloud_img, (200, 300))
sun_img = pygame.transform.scale(sun_img, (150, 150))


cat_width, cat_height = cat_img.get_rect().size
cat_x, cat_y = 50, HEIGHT - cat_height - 50
cat_y_original = cat_y

cat_jump = -10
gravity = 0.5
is_jumping = False


obstacle_width, obstacle_height = dog_img.get_rect().size
obstacle_x = WIDTH
obstacle_y = HEIGHT - obstacle_height - 50
obstacle_speed = 5

catnip_width, catnip_height = catnip_img.get_rect().size
catnip_x = WIDTH
catnip_y = HEIGHT - catnip_height - 50
catnip_speed = 3


cloud1_x, cloud1_y = 200, 100
cloud2_x, cloud2_y = 0, 70
cloud_speed = 1


sun_x, sun_y = 250, 0


score = 0
font = pygame.font.Font(None, 36)

high_score = 0
try:
    with open("highscore.txt", "r") as file:
        high_score = int(file.read())
except FileNotFoundError:
    pass


clock = pygame.time.Clock()
start_time = pygame.time.get_ticks()


game_over = True


menu_font = pygame.font.Font(None, 40)
menu_text = menu_font.render("Hi Friend:) Press SPACE to Start", True, WHITE)
menu_rect = menu_text.get_rect(center=(WIDTH // 2, HEIGHT // 2))


while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            with open("highscore.txt", "w") as file:
                file.write(str(high_score))
            pygame.quit()
            sys.exit()
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                if game_over:
                    
                    cat_x, cat_y = 50, HEIGHT - cat_height - 50
                    obstacle_x = WIDTH
                    obstacle_y = HEIGHT - obstacle_height - 50
                    catnip_x = WIDTH
                    catnip_y = HEIGHT - catnip_height - 50
                    score = 0
                    start_time = pygame.time.get_ticks()
                    game_over = False
                elif not is_jumping:
                    is_jumping = True

    if not game_over:
      
        if is_jumping:
            cat_y += cat_jump
            cat_jump += gravity
            if cat_y >= cat_y_original:
                cat_y = cat_y_original
                is_jumping = False
                cat_jump = -14

       
        obstacle_x -= obstacle_speed
        if obstacle_x < 0:
            obstacle_x = WIDTH
            obstacle_y = HEIGHT - obstacle_height - 50
            score += 1

        
        catnip_x -= catnip_speed
        if catnip_x < 0:
            catnip_x = WIDTH
            catnip_y = HEIGHT - catnip_height - 25

        
        cloud1_x -= cloud_speed
        cloud2_x -= cloud_speed
        if cloud1_x <= -100:
            cloud1_x = WIDTH
        if cloud2_x <= -100:
            cloud2_x = WIDTH


        
        if cat_x + cat_width > catnip_x and cat_x < catnip_x + catnip_width and cat_y + cat_height > catnip_y:
            catnip_x = WIDTH
            catnip_y = HEIGHT - catnip_height - 50 
            score += 1

        
        if cat_x + cat_width > obstacle_x and cat_x < obstacle_x + obstacle_width and cat_y + cat_height > obstacle_y:
            game_over = True
            if score > high_score:
                high_score = score

        
        screen.fill(PINK)

        
        screen.blit(grass_img, (0, HEIGHT - 125))

        
        screen.blit(cloud_img, (cloud1_x, cloud1_y))
        screen.blit(cloud_img, (cloud2_x, cloud2_y))
        screen.blit(sun_img, (sun_x, sun_y))

      
        screen.blit(cat_img, (cat_x, cat_y))

       
        screen.blit(dog_img, (obstacle_x, obstacle_y))

        
        screen.blit(catnip_img, (catnip_x, catnip_y))

       
        score_text = font.render("Score: " + str(score), True, WHITE)
        screen.blit(score_text, (10, 10))

        
        high_score_text = font.render("High Score: " + str(high_score), True, WHITE)
        screen.blit(high_score_text, (10, 50))

       
        current_time = pygame.time.get_ticks()
        elapsed_time = (current_time - start_time) // 1000
        clock_text = font.render("Time: " + str(elapsed_time) + "s", True, WHITE)
        screen.blit(clock_text, (10, 90))

    else:
        
        screen.fill(PINK)
        screen.blit(menu_text, menu_rect)

    
    pygame.display.flip()

    
    clock.tick(60)




       


