
# Plugin for thirdparty report

This is the guide for thirdparty report plugin support

## Document generation

using sphinx style please refer to [sphinx](http://www.sphinx-doc.org/en/master/)

```
pip install Sphinx
```

## How to add a new plugin

1. create a folder in plugin
2. modify the plugin/__init__.py to add your plugin folder name
3. implement two function in your class and export them in __init__.py of your created folder
   for example:
```
def get_plugin_name():
	logging.info('check nxp plugin')
	return "nxp"

def get_class_name():
	return "NXP"
```
4. modify the report.py add one line at import plugins, e.g.
```
import plugins
from plugins import nxp
```

5. references
* please refer to nxp folder for examples
* a helper base class testrail.py is provide for easy of use 

## Prerequest

you will need an account in testrails to access

see below code in nxpplugin.py

```
class NXP(TestRail):
	'''
    NXP extension of test rails plugin
	'''
	def __init__(self, argparser, user = "hake.huang@nxp.com"):
		reload(sys)  
		sys.setdefaultencoding('utf8')   
		logging.info('init nxp plugin')
		coloredlogs.install()
		iargparser = parse_args()
		if iargparser.token == None:
			logging.info('load security token from local')
			with open(os.path.join(os.path.dirname(os.path.realpath(__file__)),"token.txt")) as file:
				iargparser.token = file.read()

```

replace the user to your account and put a token.txt file in the nxp folder

## Run

please use this command to check your plugin functions
```
python ./report.py -P nxp -m 0 -B "Zephyr Sandbox,batch_plan,frdm_k64f,Master,kernel.device.dummy_device,1,log.txt" 

python ./report.py -P nxp -m 4 -B "Zephyr Sandbox,v2.1.0,frdm_k64f,Master,junit_report.xml,a,b"
#ignore the a,b this can be random string

python3 ./report.py -P nxp -m 0 -B "Zephyr Project - RTOS,v2.1.0,frdm_k64f,Zephyr Core,arch.common.xip,1,log.txt"

python3 ./report.py -P nxp -m 4 -B "Zephyr Project - RTOS,v2.1.0,frdm_k64f,Zephyr Core,junit_report.xml,a,b"
#ignore the a,b this can be random string
```

## Test

```
python ./report.py -P nxp -T 1
```

## query information

### query projects
```
python ./report.py -P nxp -q projects

```
### query boards
```
python ./report.py -P nxp -q boards

```
### query test suites
```
python ./report.py -P nxp -q suites

```
### query status id
```
python ./report.py -P nxp -q status_ids

```
### query cases
```
python3 ./report.py -P nxp -p 5 -q cases

```
in this case a data.yml will be generated 

### query case ref id

```
python ./report.py -P nxp -q "kernel.device.dummy_device"
```
### query section id
```
python ./report.py -P nxp -q "kernel.device"
```
### query board id
```
python ./report.py -P nxp -q "frdm_k64f"

```

## get help
ignore the error
```
python ./report.py -P nxp -H
```
