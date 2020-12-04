import pygame
import random
import math



RUNNING = True
TitleScreen = True

pygame.init()
clock = pygame.time.Clock()

width = 1780
height = 860
deathcount = '0'
wincount = '0'
highestspeed = '7'

with open("highestspeed_alltime.txt", "r") as f:
    alltime = f.read()

restart_delay = 121

myfont = pygame.font.SysFont('Comic Sans MS', 30, bold=True)
screen = pygame.display.set_mode((width,height))
pygame.display.set_caption("Pong")
pongsound = pygame.mixer.Sound('PingPongSoundEffect.ogg')
pongsound.set_volume(0.2)


ball = pygame.Rect(width/2-15, height/2-15, 30, 30,)
player = pygame.Rect(width-20,height/2-70,10,140)
oponent = pygame.Rect(10,height/2-70,10,140)

title_bar = pygame.Rect(0, 2*height/3, width, 140)
title_bar_text = myfont.render("Press Space To Begin!", False, (0,0,0))

ball_speed_x = 7 * random.choice((1,-1))
ball_speed_y = 7 * random.choice((1,-1))
player_speed_y = 7
player_speed_x = 7
oponent_speed = 7
lastballposleft = 0
lastballposright = 0
lastballposy = 0

def alltime_highestspeed():
    global alltime
    with open("highestspeed_alltime.txt", "w") as f:
        f.write(highestspeed)
        alltime = highestspeed

#Handles acceleration of ball after it hits a paddle
def acceleration():
    global ball_speed_y, ball_speed_x, highestspeed
    ball_speed_x *= 1.1
    ball_speed_y *= 1.1
    if abs(ball_speed_x) > int(highestspeed):
        highestspeed = str(math.ceil(abs(ball_speed_x)))
    if abs(ball_speed_x) > int(alltime): #cont[1] is the highest speed all time that's written in the file
        alltime_highestspeed()

#Checks if due to the speed of the ball if the ball 'phases' through the paddle. returns true if it does.
def phase():
    if lastballposleft >= oponent.left and ball.left <= oponent.left and ball_speed_x < 0 and oponent.bottom > (lastballposy+15) + (ball_speed_y/abs(ball_speed_y))*(lastballposleft - oponent.right) > oponent.top:
        return True
    if lastballposright <= player.right and ball.right >= player.right and ball_speed_x > 0 and player.bottom > (lastballposy+15) + (ball_speed_y/abs(ball_speed_y))*(player.left-lastballposright) > player.top:
        return True
    else:
        return False

#Handles ball movements, bouncing, and losses
def ballAnimation():
    global ball_speed_y, ball_speed_x, wincount, deathcount, lastballposleft, lastballposright, lastballposy
    lastballposy = ball.y
    lastballposleft = ball.left
    lastballposright = ball.right

    ball.x += ball_speed_x
    ball.y += ball_speed_y

    if (ball.top <=0 and ball_speed_y<0) or (ball.bottom >=height and ball_speed_y>0):
        pongsound.play()
        ball_speed_y *= -1
    elif phase():
        pongsound.play()
        ball_speed_x *= -1
        acceleration()
    elif ball.left <= 0 and not phase():
            wincount = str(int(wincount)+1)
            ballRestart()
            return True
    elif ball.right >= width and not phase():
            deathcount = str(int(deathcount)+1)
            ballRestart()
            return True
    elif (ball.colliderect(player) and ball_speed_x > 0) or (ball.colliderect(oponent) and ball_speed_x < 0):
        pongsound.play()
        ball_speed_x *= -1
        acceleration()
        
#handles player movement (used in pause)
def playerAnimation():
    player.y += player_speed_y
    player.x += player_speed_x
    if player.top < 0:
        player.top = 0
    if player.bottom > height:
        player.bottom = height
    if player.right > width:
        player.right = width
    if player.left < width/2:
        player.left = width/2

#animates the player in the title screen to randomly move
def title_player_animation():
    global player_speed_x, player_speed_y
    player.y += player_speed_y
    player.x += player_speed_x
    player_speed_y *= random.choice((1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,-1))
    player_speed_x *= random.choice((1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,-1))
    if player.top < 0:
        player.top = 0
    if player.bottom > height:
        player.bottom = height
    if player.right > width:
        player.right = width
    if player.left < width/2:
        player.left = width/2

#handles the oponent AI to predict where the ball will go

def oponentAnimation():
    if ball_speed_y > 0 and ball_speed_x < 0: #Is ball going down?
        if height - ball.bottom < ball.left: #Bounce atleast once?
            if height < ball.left-(height - ball.bottom): #Bounce twice?
                if oponent.top > ball.left - (height - ball.bottom) - height:
                    oponent.top -= oponent_speed
                if oponent.bottom < ball.left - (height - ball.bottom) - height:
                    oponent.bottom += oponent_speed
            else:                                   #Bounce only once
                if oponent.top > height - (ball.left-(height - ball.bottom)):
                    oponent.top -= oponent_speed
                if oponent.bottom < height - (ball.left-(height - ball.bottom)):
                    oponent.bottom += oponent_speed
        else:                                   #Doesn't bounce
            if oponent.top > ball.left+ball.top:
                oponent.top -= oponent_speed
            if oponent.bottom < ball.left+ball.top:
                oponent.bottom += oponent_speed
                 
    if ball_speed_y < 0 and ball_speed_x < 0: #Is the ball going up?
        if ball.top < ball.left:               #Bounce atleast once?
            if height < ball.left-ball.top:     #Bounce twice?
                if oponent.top > height- (ball.left-ball.top-height):
                    oponent.top -= oponent_speed
                if oponent.bottom < height - (ball.left-ball.top-height):
                    oponent.bottom += oponent_speed
            else:                               #Bounce only once              
                if oponent.top > ball.left-ball.top:
                    oponent.top -= oponent_speed
                if oponent.bottom < ball.left-ball.top:
                    oponent.bottom += oponent_speed
        else:                                   #Doesn't bounce
            if oponent.top > ball.bottom-ball.left:
                oponent.top -= oponent_speed
            if oponent.bottom < ball.bottom-ball.left:
                oponent.bottom += oponent_speed

    if oponent.bottom > height:
        oponent.bottom = height
    if oponent.top < 0:
        oponent.top = 0

#resets the ball to the middle
def ballRestart():
    global ball_speed_x, ball_speed_y, restart_delay
    ball.center = (width/2, height/2)
    ball_speed_y = 0
    ball_speed_x = 0 
    restart_delay = 0

#handles the delay between the restart and then gameplay
def restartDelay():
    global ball_speed_y, ball_speed_x, restart_delay
    if restart_delay < 120:
        restart_delay += 1
    if restart_delay == 120:
        ball_speed_x = 7 * random.choice((1,-1))
        ball_speed_y = 7 * random.choice((1,-1))
        restart_delay+=1

def pause():
    global player_speed_x, player_speed_y
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_ESCAPE:
                    return
                if event.key == pygame.K_DOWN:
                    player_speed_y += 7
                if event.key == pygame.K_UP:
                    player_speed_y += -7
                if event.key == pygame.K_LEFT:
                    player_speed_x += -7
                if event.key == pygame.K_RIGHT:
                    player_speed_x += 7
            if event.type  == pygame.KEYUP:
                if event.key == pygame.K_DOWN:
                    player_speed_y += -7
                if event.key == pygame.K_UP:
                    player_speed_y += 7
                if event.key == pygame.K_LEFT:
                    player_speed_x += 7
                if event.key == pygame.K_RIGHT:
                    player_speed_x += -7
                    
while TitleScreen:

    oponentAnimation()
    title_player_animation()
    ballAnimation()

    for event in pygame.event.get():
        if event.type== pygame.QUIT:
            TitleScreen = False
            RUNNING = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE: 
                ballRestart() 
                player_speed_x = 0
                player_speed_y = 0
                deathcount = '0'
                wincount = '0'
                highestspeed = '7'
                player = pygame.Rect(width-20,height/2-70,10,140)
                TitleScreen = False
    
    screen.fill((100,100,100))
    pygame.draw.rect(screen, (255,255,255), player)
    pygame.draw.rect(screen, (255,255,255), oponent)
    pygame.draw.ellipse(screen, (255,0,0), ball)
    pygame.draw.aaline(screen, (255,0,0), (width/2,0), (width/2,height))
    pygame.draw.rect(screen, (255,255,255), title_bar)
    screen.blit(title_bar_text, (width/3, 2*height/3+35))
    
    restartDelay()

    pygame.display.flip()
    clock.tick(60)




while RUNNING:
    for event in pygame.event.get():
        if event.type== pygame.QUIT:
            RUNNING = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_DOWN:
                player_speed_y += 7
            if event.key == pygame.K_UP:
                player_speed_y += -7
            if event.key == pygame.K_LEFT:
                player_speed_x += -7
            if event.key == pygame.K_RIGHT:
                player_speed_x += 7
            if event.key == pygame.K_ESCAPE:
                pause()    
        if event.type  == pygame.KEYUP:
            if event.key == pygame.K_DOWN:
                player_speed_y += -7
            if event.key == pygame.K_UP:
                player_speed_y += 7
            if event.key == pygame.K_LEFT:
                player_speed_x += 7
            if event.key == pygame.K_RIGHT:
                player_speed_x += -7
            
            

    ballAnimation()
    playerAnimation()
    oponentAnimation()

    alltimedisplay = myfont.render("All Time Highest Speed: " + alltime, False, (200,200,255))
    windisplay = myfont.render(wincount, False, (200,255,200))
    deathdisplay = myfont.render(deathcount, False, (255,200,200))
    speeddisplay = myfont.render(highestspeed, False, (200,200,255))

    screen.fill((100,100,100))
    screen.blit(deathdisplay, (50,20))
    screen.blit(windisplay,(width-50,20))
    pygame.draw.rect(screen, (255,255,255), player)
    pygame.draw.rect(screen, (255,255,255), oponent)
    pygame.draw.ellipse(screen, (255,0,0), ball)
    pygame.draw.aaline(screen, (255,0,0), (width/2,0), (width/2,height))
    screen.blit(speeddisplay, (width/2-(8*len(highestspeed)),20))
    screen.blit(alltimedisplay, (width/2 - (8*len("All Time Highest Speed: " + alltime)), height-50)) 
    
    restartDelay()

    pygame.display.flip()
    clock.tick(60)

pygame.quit()
