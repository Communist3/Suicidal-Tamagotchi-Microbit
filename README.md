# Tamagotchi-Microbit
This is an old school tamagotchi remade using a BBC Microbit. 
The code:                     

from microbit import *
import random
import music
happyFace=Image("00000:"
                "09090:"
                "00000:"
                "90009:"
                "99999")
happyLeft=Image("00000:"
                "09090:"
                "00000:"
                "90009:"
                "99999")
happyRight=Image("00000:"
                 "00909:"
                 "00000:"
                 "90009:"
                 "99999")
unhappyFace=Image("00000:"
              "09090:"
              "00000:"
              "99999:"
              "90009")
deadFace=Image("00000:"
               "99099:"
               "90009:"
               "00000:"
               "99999")
tick=Image("00000:"
           "00009:"
           "00090:"
           "90900:"
           "09000")
cross=Image("90009:"
            "09090:"
            "00900:"
            "09090:"
            "90009")
lvlUp=Image("90090:"
            "90999:"
            "90090:"
            "90090:"
            "99090")
happy=True
hungry=False
sleepy=False
death=False
eyeMovementState=0
deathTimer=1000
eyeMoveChance=0
eyeMoveState=0
moodChance=0
expCap=100
expCount=0
level=1

while True:
    if expCount>=expCap:
        level+=1
        expCount=0
        expCap=expCap*2
        display.show(lvlUp)
        sleep(200)

    eyeMoveChance=random.randint(0,100)
    if eyeMoveChance<=10:
        eyeMovementState=2
    elif eyeMoveChance>10 and eyeMoveChance<=20:
        eyeMoveState=1
    else:
        eyeMoveState=0
    
    if happy==True:
        if eyeMoveState==0:
            display.show(happyFace)
        elif eyeMoveState==1:
            display.show(happyLeft)
        else:
            display.show(happyRight)
        sleep(2000)

    if happy==True:
        moodChance=random.randint(1,50-level)
    
    if moodChance<=1 and moodChance<=10:
        happy=False
        hungry=True
        sleepy=False
    elif moodChance>=10 and moodChance<=20:
        happy=False
        hungry=False
        sleepy=True
    else:
        happy=True
        hungry=False
        sleepy=False
    
    if (hungry==True or sleepy==True) and deathTimer==0:
        death=True
    
    if hungry==True or sleepy==True:
        display.show(unhappyFace)
        deathTimer-=1
    
    if death==True:
        display.show(deadFace)
        music.play(music.FUNERAL)
        sleep(8000)
        display.scroll("Game Over, you were unable to care for it")

    if button_a.is_pressed():
        if hungry==True:
            happy=True
            hungry=False
            sleepy=False
            deathTimer=1000
            expCount+=100
            display.show(tick)
            sleep(1000)
        else:
            display.show(cross)
            deathTimer-=5
            sleep(1000)
    
    if button_b.is_pressed():
        if sleepy==True:
            happy=True
            hungry=False
            sleepy=False
            deathTimer=1000
            expCount+=100
            display.show(tick)
            sleep(1000)
        else:
            display.show(cross)
            deathTimer-=5
            sleep(1000)


