# Chat Cool Down Feature for BombSquad 1.4.150+

This mod feature can make a cool down for people to chat in your BombSquad server.
Since i have provided white (whitelist for cooldown)... Add admins/owners, so that admins can use some useful commands like '/kick' on frikin ppl
in server instantly.. umm server editor can make this feature to only available owners too, if needed.

Note: If you don't know English or can't understand my language.. simply go to YouTube and learn English... And one main thing, please let me know if these codes have error or spelling mistake as I am not that much expert, so that i can fix it !


# Codes of Main Process

The below codes should be placed in bsUI.py's filterChatMessage() function (maybe line no. 23827 i guess) as like below...

    chatCoolDown = {}
    chatCoolDownTime = 5 #In Seconds
    white = ['\xee\x80\xa0FireFighter1027'] #just an example on how to add..  remove my if u want...
    def _filterChatMessage(msg, clientID):
        global chatCoolDown #in case if needed
        global chatCoolDownTime #in case if needed 
        time = int(bs.getRealTime())
        for i in bsInternal._getGameRoster():
            if (clientID != -1) and (i != {}) and (i['clientID'] == clientID):
                #Some Useful Variables
                dis_str = i['displayString']
                name = i['players'][0]['name']
                clID = i['clientID']
                if (i['players'] == []) or (not i['players']):
                    bs.screenMessage("Join the Game First!", color=(1,0,0), clients=[clID], transient=True)
                    return None
                if (dis_str not in white) and (dis_str in chatCoolDown) and (time < chatCoolDown[dis_str]):
                    bal = int(chatCoolDown[dis_str] - time) / 1000
                    bs.screenMessage("Too Fast, you have {} second(s) coolDown...".format(str(bal)), color=(1,0,0), clients=[clID], transient=True)
                    return None
                chatCoolDown[dis_str] = time + (chatCoolDownTime * 1000)
                return msg


# Codes for managing chatCD through commands

The below codes should be added in chatCmd.py or cheatCmd.py (executes things throught chat commands).

Go to YouTube and Learn what is Indendation in python if you don't know, as u need to edit the below thing's indendation.

    elif m == '/cd':
        #replace the 'bsUI' if you are using a custom file for that like chatFilter.py 
        #It must be the file name where you put the main process codes given above
        import bsUI
        try:
            if a[0].lower() in ('no', 'off', 'disable'):
                if bsUI.chatCoolDownTime:
                    bsUI.chatCoolDownTime = False
                    #commandSuccess = True #This line maybe used by you in other commands
                else: bs.screenMessage("Wait what, why u wanna disable a thing\n which is already disabled..?", color=(1,0,0), clients=[clientID], transient=True)
            else:
                try:
                    if int(a[0].lower()) in range(300):
                        bsUI.chatCoolDownTime = int(a[0])
                        bsInternal._chatMessage("Successfully set chatCoolDown time to {} seconds :)".format(str(a[0])))
                        #commandSuccess = True #This line maybe used by you in other commands
                    else: bs.screenMessage("Oof... 300 seconds is maximum cooldown, Why this much?", color=(1,1,1), clients=[clientID], transient=True)
                except:
                    bs.screenMessage("Give an Integer as arg... you can't trick me\n Usage: '/cd CD_Time_In_Integer'", color=(1,0,0), clients=[clientID], transient=True)
        except:
            bs.screenMessage("Usage:\n'/cd disable/off/no' [or] '/cd CD_Time_In_Integer' for enabling...", color=(1,0,0), clients=[clientID], transient=True)


# Credits
If you try taking/stealing credits for the above beautiful works by me, then you are gae!
