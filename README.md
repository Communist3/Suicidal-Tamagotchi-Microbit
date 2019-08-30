# Tamagotchi-Microbit
This is an old school tamagotchi remade using a BBC Microbit. The game involves a user trying to sustain and keep a creature alive. The creature will get sleepy and tired, and the player must feed and put the creature to bed to make it happy again. If it remains unhappy for too long, the creature will die and the game will be over, it also gets progressively more difficult as the creature leveles up and the game continues. We have made use of the two onboard buttons for the microbit. The button on the left(ButtonA) feeds the creature, while the button on the right(ButtonB) puts the creature to sleep. This was not an original idea, so we modified it by giving it a leveling system as well as tweaked the difficulty of the overall code, as well as added a diffferent death face, added music and a custom death text.<br>
The code:                     

from microbit import * <br>
import random <br>
import music <br>
happyFace=Image("00000:" <br>
                "09090:" <br>
                "00000:" <br>
                "90009:" <br>
                "99999") <br>
happyLeft=Image("00000:" <br>
                "09090:" <br>
                "00000:" <br>
                "90009:" <br>
                "99999") <br>
happyRight=Image("00000:" <br>
                 "00909:" <br> 
                 "00000:" <br>
                 "90009:" <br>
                 "99999") <br>
unhappyFace=Image("00000:" <br>
              "09090:" <br>
              "00000:" <br>
              "99999:" <br>
              "90009") <br>
deadFace=Image("00000:" <br>
               "99099:" <br>
               "90009:" <br>
               "00000:" <br>
               "99999") <br>
tick=Image("00000:" <br>
           "00009:" <br>
           "00090:" <br>
           "90900:" <br>
           "09000") <br>
cross=Image("90009:" <br>
            "09090:" <br>
            "00900:" <br>
            "09090:" <br>
            "90009") <br> 
lvlUp=Image("90090:" <br>
            "90999:" <br>
            "90090:" <br>
            "90090:" <br> 
            "99090") <br>
happy=True <br>
hungry=False <br>
sleepy=False <br>
death=False <br>
eyeMovementState=0 <br>
deathTimer=1000 <br>
eyeMoveChance=0 <br>
eyeMoveState=0 <br>
moodChance=0 <br>
expCap=100 <br>
expCount=0 <br>
level=1 <br>

while True: <br>
    if expCount>=expCap: <br>
        level+=1 <br>
        expCount=0 <br>
        expCap=expCap*2 <br>
        display.show(lvlUp) <br>
        sleep(200) <br>

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

Testing Methods:<br>
We went about testing the code by first setting the creature to be unhappy primarily, and seeing how long it would take to die. We then adjusted values accordingly until we were happy with the balance in the difficulty.We also tested the chances of the creature becoming unhappy and how that would affect gameplay. Errors that arose during testing were that the creature was perpetually happy, thus providing no difficulty to the game, thus making it unfun to play, we corrected this by putting all of the code in a forever loop.<br><br>
Extensions:<br>
We thought that it could be fun to add minigames, which would be played against the creature to give buffs and/or debuffs to the creature based off of the game and the performance of the player, for example if the creature was tired when a game is played, then once the game is over, it will be more likely to become tired more often. Another idea would be to give the creature a voice and a name, thus making it more personal and emotional if it dies.<br><br>
The source for the original idea: https://static1.squarespace.com/static/533a5f1be4b00bb34469c085/t/5ae6f3c00e2e72dfd92a15dd/1525085122048/Tamagotchi.pdf 
