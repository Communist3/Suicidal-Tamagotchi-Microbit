# Tamagotchi-Microbit
This is an old school Tamagotchi remade using a BBC Microbit. The player must attempt to keep a digital pet alive for as long as possible. The user must feed the creature and put it to sleep, when required, in order to keep it alive. When the correct action is done, the creature gains experience and levels up when the level requirement is met. If the creature is not looked after, it will eventually die and the game will end.<br> 
The code:                     
```python
from microbit import *
import random
import music
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
hungryFace=Image("00000:"
              "09090:"
              "00000:"
              "99999:"
              "90009")
sleepyFace=Image("00000:"
                 "99099:"
                 "00000:"
                 "99999:"
                 "00000")
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
deathTimer=5000
eyeMoveChance=0
eyeMoveState=0
moodChance=0
expCap=100
expCount=0
level=1
levelCap=450

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

```
Testing Information:<br>
In order to test the code, we tested how the code would react if the creature started out as either sleepy or hungry, the code was able to handle this, and once the correct response was given, the code functioned as normal. We also tested the difficulty of the game by adjusting how high or low the time taken for the creature to die will be, in order to make the game not too easy, as well as not too difficult. We also tested how often the creature will become sleepy or hungry in order to make the game entertaining as well as have some challenge to it. The main bugs we encountered in creating the project was that the creature would not change its emotion from happy, we fixed this issue by placing all the code in one forever loop.<br><br>
Possible Expansions:<br>
In order to add more content to our game, we think that adding mini-games that can be played with the pet could be a way to increase the pets death timer, or even give certain positive effects to the creature, like making the chances of it becoming hungry or sleepy be less for a short amount of time. Another way to expand the game would be to have a set of objectives for the player to have, if they get accomplished, then the player could win the game, as currently the game only ends if the pet dies.<br><br>
Original doccument:<br>
https://static1.squarespace.com/static/533a5f1be4b00bb34469c085/t/5ae6f3c00e2e72dfd92a15dd/1525085122048/Tamagotchi.pdf 
