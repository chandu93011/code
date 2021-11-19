# code
An Api used and executed python code to generate a alert message with sound while updating a  wbsite.
import requests
import time
import os
import pyttsx3

def beep(msg):
	engine.say(msg)
	engine.runAndWait()
			
URL = "https://cdn-api.co-vin.in/api/v2/appointment/sessions/public/findByPin?pincode=221602&date=19-11-2021"
center_ids = [720737, 615467, 860531, 844867]
engine = pyttsx3.init()
engine.setProperty('rate', 150)
engine.setProperty('voice', 'com.apple.speech.synthesis.voice.samantha')

while True:
	available_d1 = False
	available_d2 = False
	try:
		response = requests.get(url = URL)
	except:
		beep("some error occured")
	data = response.json()
	if response.status_code != 200:		
		print(response)
		beep("Non successful response")
	
	sessions = data['sessions']

	for session in sessions:
		if session['center_id'] in center_ids and session['available_capacity_dose1'] > 0:
			print("[{}]::".format(time.ctime()), 'Available capacity for D1 on', session['name'], "is", session['available_capacity_dose1'])
			beep("Available capacity for dose 1 is {} at {}".format(session['available_capacity_dose1'], session['address']))
			available_d1 = True
			
		if session['center_id'] in center_ids and session['available_capacity_dose2'] > 0:
			print("[{}]::".format(time.ctime()), 'Available capacity for D2 on', session['name'], "is", session['available_capacity_dose2'])

	if not (available_d1 or available_d2):
		time.sleep(60.0)
