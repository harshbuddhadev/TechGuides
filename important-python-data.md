# Important Python Commands/Snippets/Data

## Python Flask API
### Installation

```bash
python3 -m pip install Flask
```

### Usage
```python
import Flask
@app.route('/alert/<id>/<state>')
def main(id,state):
  print(f"ID : {id}")
	print(f"State : {state}")

app.run("0.0.0.0",8082) #Run API on All interfaces and on port 8082
```

## Data Logger

```python
import datetime
log_location = "/home/pi/text.txt"
log = open(log_location,"a+")
now = datetime.now()
log.write(str("\nTime:"+ str(now.strftime("%H:%M:%S"))+"\n"))
log.write(f"Log Location : {log_location} \n")
log.write("\n")
log.close()
```

## save returned list to specific variables
```python
def main():
	return ["1","2"]
[id, data]=(main()) #Fetch Data
```

## Execute Command only if script is run directly

```python
if __name__ == "__main__":
	exit()
```

## Fetch Data from Text file, while ignoring "#" commented Lines

```python
with open("/home/pi/name.txt", "r") as file:
	for line in file:
		line = line.strip()
		if line.startswith("#") or not line:
			continue
		id, loc, addr = line.split("=", 2)
		addresses[id] = addr
```

## Multithreading

```python
import threading
alert_thread = threading.Thread(target=announce, args=(),daemon=True) #create thread for the announce function
alert_thread.start() #start the thread
```

## Try Except python

```python
try:
	print("Try")
except KeyboardInterrupt:
	print("Keyboard Interupt")
	exit()
except Exception as e:
	print(f"Exception Occured : {e}")
	exit()
```

## JSON

### Load Dictionary/String to JSON Object
```python
import json
data={"A":"B","C":"D"}
rcvd_data=json.loads(data)
```
### Load JSON Object/File to Dictionary/String
```python
import json
with open('file.json') as file:
	data = json.load(file)
```