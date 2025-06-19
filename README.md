# raspberry-pi-smart-exhaust

from flask import Flask, request, render_template 
import RPi.GPIO as GPIO 
app = Flask(__name__) 
GPIO.setmode(GPIO.BCM) 
devices = {'light': 17, 'fan': 27} 
for pin in devices.values(): 
GPIO.setup(pin, GPIO.OUT) 
GPIO.output(pin, GPIO.LOW) 
@app.route('/') 
def index(): 
return render_template('index.html') 
@app.route('/control', methods=['GET']) 
def control(): 
device = request.args.get('device') 
action = request.args.get('action') 
pin = devices.get(device) 
if pin is not None: 
GPIO.output(pin, GPIO.HIGH if action == 'on' else GPIO.LOW) 
return f"{device} turned {action}" 
if __name__ == '__main__': 
app.run(host='0.0.0.0', port=80) 
