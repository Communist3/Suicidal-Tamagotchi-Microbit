# Tamagotchi-Microbit
This is an old school tamagotchi remade using a BBC Microbit.
The code:                     

from microbit import *<br>
import random<br>
import music<br>
happyFace=Image("00000:"<br>
                "09090:"<br>
                "00000:"<br>
                "90009:"<br>
                "99999")<br>
happyLeft=Image("00000:"<br>
                "90900:"<br>
                "00000:"<br>
                "90009:"<br>
                "99999")<br>
happyRight=Image("00000:"<br>
                 "00909:"<br>
                 "00000:"<br>
                 "90009:"<br>
                 "99999")<br>
hungryFace=Image("00000:"<br>
              "09090:"<br>
              "00000:"<br>
              "99999:"<br>
              "90009")<br>
sleepyFace=Image("00000:"<br>
                 "99099:"<br>
                 "00000:"<br>
                 "99999:"<br>
                 "00000")<br>
deadFace=Image("00000:"<br>
               "99099:"<br>
               "90009:"<br>
               "00000:"<br>
               "99999")<br>
tick=Image("00000:"<br>
           "00009:"<br>
           "00090:"<br>
           "90900:"<br>
           "09000")<br>
cross=Image("90009:"<br>
            "09090:"<br>
            "00900:"<br>
            "09090:"<br>
            "90009")<br>
lvlUp=Image("90090:"<br>
            "90999:"<br>
            "90090:"<br>
            "90090:"<br>
            "99090")<br>
happy=True<br>
hungry=False<br>
sleepy=False<br>
death=False<br>
eyeMovementState=0<br>
deathTimer=5000<br>
eyeMoveChance=0<br>
eyeMoveState=0<br>
moodChance=0<br>
expCap=100<br>
expCount=0<br>
level=1<br>
levelCap=450<br><br>

display.scroll("Press A to feed the pet, Press B to put it to sleep")
while True:
    if expCount>=expCap:
        if level!=levelCap:
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
        moodChance=random.randint(1,500-level)
    
    if moodChance>=1 and moodChance<=10:
        happy=False
        hungry=True
        sleepy=False
    elif moodChance>10 and moodChance<=20:
        happy=False
        hungry=False
        sleepy=True
    else:
        happy=True
        hungry=False
        sleepy=False
    
    if (hungry==True or sleepy==True) and deathTimer==0:
        death=True
    
    if hungry==True:
        display.show(hungryFace)
        deathTimer-=1
    
    if sleepy==True:
        display.show(sleepyFace)
        deathTimer-=1
    
    if death==True:
        display.show(deadFace)
        music.play(music.FUNERAL)
        sleep(8000)
        display.scroll("Game Over, you reached level " + str(level) + ", you were unable to care for your pet")

    if button_a.is_pressed():
        if hungry==True:
            happy=True
            hungry=False
            sleepy=False
            deathTimer=5000
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
            deathTimer=5000
            expCount+=100
            display.show(tick)
            sleep(1000)
        else:
            display.show(cross)
            deathTimer-=5
            sleep(1000)

