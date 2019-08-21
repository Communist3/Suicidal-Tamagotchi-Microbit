# Tamagotchi-Microbit
This is an old school tamagotchi remade using a BBC Microbit.
The code:
from microbit import *
import random
happyFace=Image("00000:"
                "09090:"
                "00000:"
                "90009:"
                "99999")

happyLeft=Image("00000:"
                "90900:"
                "00000:"
                "90009:"
                "99999")

happyRight=Image("00000:"
                 "00909:"
                 "00000:"
                 "90009:"
                 "99999")

unhappy=Image("00000:"
              "09090:"
              "00000:"
              "99999:"
              "90009")

deadFace=Image("90009:"
               "09090:"
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

happy=True
hungry=False
sleepy=False
dead=False
eyeMovementState=0
deathTimer=100
eyeMoveChance=0
eyeMoveState=0

while True:
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

while True:
    if happy==True:
        moodChance=random.ranint(0,5000)
    
    if moodChance<=1:
        happy=False
        hungry=True
        sleepy=False
    elif moodChance>1 and moodChance<=3:
        happy=False
        hungry=False
        sleepy=True
    else:
        happy=True
        hungry=False
        sleepy=False
    
    if (hungry==True or sleepy==True) and deathTimer==0:
        dead=True
        #add music later
    
    if hungry==True or sleepy==True:
        display.show(unhappy)
        deathTimer-=1
    
    if death==True:
        display.show(dead)
        sleep(8000)
        display.scroll("Game Over, you are a bad parent")

while True:
    if button_a.is_pressed():
        if hungry==True:
            happy=True
            hungry=False
            sleepy=False
            deathTimer=100
            display.show(tick)
        else:
            display.show(cross)

    if button_b.is_pressed():
        if sleepy==True:
            happy=True
            hungry=False
            sleepy=False
            deathTimer=100
            display.show(tick)
        else:
            display.show(cross)
