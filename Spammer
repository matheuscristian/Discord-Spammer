#!/bin/python3

from os.path import exists
from configparser import ConfigParser
from requests import post
from random import randint
from time import sleep

def main():
    config = ConfigParser()
    # Create file with default options if it doesn't exists
    if not exists("config.ini"):
        config["General"] = {
            "Token": "None",
            "Delay": 1.0
        }
        with open("config.ini", "w") as config_file:
            config.write(config_file)
    # If the file exists, read and parse it
    else:
        config.read_file(open("config.ini", "r"))

    if not config["General"]["Token"] or config["General"]["Token"] == "None":
        raise BaseException("Missing 'Token' property in config file")

    # If message file doesn't exists create it with a default message
    if not exists("message.txt"):
        with open("message.txt", "w") as message_file:
            message_file.write("Example message")

    # Read the message file
    message = ''
    with open("message.txt", "r") as message_file:
        message = '\n'.join(message_file.readlines())
    
    channel_id = input("Enter channel id: ")
    
    if config["General"]["Delay"]:
        delay = float(config["General"]["Delay"])
    else:
        delay = 0.5

    # Loop that keeps sending the message
    while True:
        post(f"https://discord.com/api/v9/channels/{channel_id}/messages", 
            headers={
                "Authorization": config["General"]["Token"]
            },
            json={
                "content": message,
                "nonce": ''.join([str(randint(0, 9)) for _ in range(18)]),
                "tts": "false"
            })
        sleep(delay)

if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print()
        exit(0)
