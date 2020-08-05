# Installing Py4Web on pythonanywhere from source

Having installed py4web on pythonanywhere using pip as per https://youtu.be/Wxjl_vkLAEY I recently had the need to run py4web from source for dev purposes.
The initial install was very much vanilla with all defaults accepted from the pythonanywhere web tab and ended up like this:
```
Code:
What your site is running.

Source code:
/home/eudorajab/

Working directory:
/home/eudorajab/

WSGI configuration file:/var/www/eudorajab_pythonanywhere_com_wsgi.py

Python version:3.8
```
This creates the bottle_app.py, password.txt and the apps folder in the /home/eudorajab directory. 

NOTE: The bottle_app.py file after editing shoould look like this: 
```
"""
Documented here: https://youtu.be/Wxjl_vkLAEY
"""
import os
from py4web.core import wsgi
PASSWORD_FILENAME = 'password.txt'
DASHBOARD_MODE = 'full' or 'demo' or 'none'
APPS_FOLDER = 'apps'

password_file = os.path.abspath(os.path.join(os.path.dirname(__file__), PASSWORD_FILENAME))
application = wsgi(password_file=password_file,
                   dashboard_mode=DASHBOARD_MODE,
                   apps_folder=APPS_FOLDER)

```
In order to run Py4Web from source I did the following :-
```
In Pythonanywhere open a new bash console
git clone https://github.com/web2py/py4web.git
cd py4web
python3 -m pip install -r requirements.txt
./py4web.py setup apps
./py4web.py set-password
as per the py4web README
```
This will create a py4web folder in ```/home/<username>``` and in our example looked like /home/eudorajab/py4web
 
Next step is to move the bottle_app.py file from the /home/eudorajab/ folder into the /home/eudorajab/py4web folder

Next from the web tab in pythonanywhere change
```
Source code:
/home/eudorajab/py4web/

Working directory:
/home/eudorajab/py4web/
```
You can do this by clicking on the actual field and changing

Next click on your WSGI configuration link which will allow you to edit this file

Your edited file should look something like this :-
```
# This file contains the WSGI configuration required to serve up your
# web application at http://<your-username>.pythonanywhere.com/
# It works by setting the variable 'application' to a WSGI handler of some
# description.
#
# The below has been auto-generated for your Bottle project
import bottle
import os
import sys

# add your project directory to the sys.path
project_home = '/home/eudorajab/py4web/apps'
if project_home not in sys.path:
    sys.path = [project_home] + sys.path

# make sure the default templates directory is known to Bottle
templates_dir = os.path.join(project_home, 'views/')
if templates_dir not in bottle.TEMPLATE_PATH:
    bottle.TEMPLATE_PATH.insert(0, templates_dir)

# import bottle application
from bottle_app import application
```
NOTE: project_home needs to point to your apps foleder. In this case it was originally /home/eueorajab/

Reload the server from the Web Tab and that should be it.



