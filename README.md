---server.py

import socket

abbreviations={"tbh":"to be honest","idc":"I don't care","ig":"I guess",
"tbf":"to be fair","atm":"at the moment","irl":"in real life",
"lol":"laughing out loud","asap":"as soon as possible",
"omg":"oh my God","ttyl":"talk to you later",
"idk":"I don't know","nvm":"never mind"}

def translate(sentence):
    words=sentence.split()
    translated_words=[]
    for word in words:
        translated_word=abbreviations.get(word.lower(),word)
        translated_words.append(translated_word)
    return " ".join(translated_words)

s=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
s.bind(("0.0.0.0",8080))
print("UDP Server Running...")
data,addr=s.recvfrom(4096)
sentence=data.decode()
print("Received sentence:")
print(sentence)
s.sendto(translate(sentence).encode(),addr)
print("Translated message sent.")
s.close()

---client.py

import socket

s=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
addr=("127.0.0.1",8080)
msg=input("Enter a new-generation English sentence: ")
s.sendto(msg.encode(),addr)
data,addr=s.recvfrom(4096)
print("Translated sentence:")
print(data.decode())
s.close()
