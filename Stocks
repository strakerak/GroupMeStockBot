
#This is how to use a GroupMe bot to output the price of a stock to a group chat through commands.

#YOU WILL NEED A GROUPME BOT FOR THIS. THIS IS HOW YOU GET ONE
#1) Go to dev.groupme.com
#2) Log in. Use your auth code sent to your phone or email
#3) Click on "Bots" and "Create Bot"
#4) Select the Group you want to place it in
#5) Type the name of your bot
#6) For the callback URL, this will be the place that you host your bot. I use Repl.It most of the time, and for many other projects. Heroku works as well, but you might have to change the IP address on the bottom for hosting
#7) Select an avatar for your bot, and create it!
#8) Most importantly, copy the BOT ID to the "botid" parameter below. If you don't, you won't be able to send messages.

#Side note, you might have to add ".robinhood" after r. Some verions of Python don't support the ".robinhood" after r, and some do. It is up to what you are using. This is written for Repl.It and Python 3.9

import re
import os
import sys
import robin_stocks as r

from flask import Flask, request
import requests

#You will need to put your login information from Robinhood here
USERNAME=""
PASSWORD=""

timer=3600*24*30 #This will set your auth time to 30 days. This might be shorter, and Robinhood might kick you out if you're using some kind of online bot host like Repl.It

login=r.login(USERNAME,PASSWORD,expiresIn=timer)

app = Flask(__name__)
@app.route('/',methods=['POST']) #don't touch this, this is standard GroupMe stuff
def webhook(): #This is message analysis logic. Pretty much reads in what you post.
  botid = "887c985d090c2d2669c74b8ace" #You will get this from the GroupMe Dev page.
  data = request.get_json()
  log('Data is {}'.format(data))
  print(data['group_id']) #This will tell you what group has been read. Only really necessary if your bot is in multiple groups, so it knows where to send back to.
  groups=[] #You don't need this if you are only using one Group. If you are using more than one, make this a list of lists where it is as follows [[group_id,bot_id]]. You will need to place the Bot ID in yourself. GroupMe will not display this data in the request.
  groupid=data['group_id']
  #I removed the portion where you for loop through to get the BotID paired with the GroupID. There is probably a better way to do it, but it isn't necessary. If you really need it, it's a simple for loop and if statement.
  
  if data['name'] != "Shasta": #Making sure the bot isn't replying to itself. You can change Shasta to anything. This is just what my bot was.
  
   message=data['text'].split()
   if message[0]=='!getstock' and len(message)==2:
    stock=message[1]
    price = r.stocks.get_latest_price(stock)
    msg = "Price for " + stock + " is " + str(float(price[0]))
    send_message(msg,botid)
   
   return "ok", 200 #Sends message


def send_message(msg,botid):
  from time import sleep
  sleep(0.1)
  url='https://api.groupme.com/v3/bots/post'
  data={'bot_id':str(botid),'text':msg}
  response=requests.post(url,params=data)

def log(msg):
  print(str(msg))
  sys.stdout.flush()


if __name__ == "__main__":
    app.run(debug=True, host='0.0.0.0')
  
