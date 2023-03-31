```python
#!python3
import winreg
def latest_installed_program(software_name, search_64_first=True, first_call = True):
	"""Try to find the latest installed version of a specified program on the system
	
	Args:
	    software_name (string): name of the software to look for (3ds Max, Autodesk Maya etc.)

	    search_64_first (int): where to check first, if you know it's going to be  winreg.KEY_WOW64_32KEY then put 
		
		first_call (bool): Don't set this manually.
	Returns:
	    dictionary: dictionary with the latest program info in it.

    Example usage:
	latest_installed_program("3ds Max", True )
	"""
	win_flag = None
	if search_64_first:
		win_flag = winreg.KEY_WOW64_64KEY 
	else:
		win_flag = winreg.KEY_WOW64_32KEY

	hive=winreg.HKEY_LOCAL_MACHINE

	aReg = winreg.ConnectRegistry(None, hive)
	icons = r"HKEY_CLASSES_ROOT\Installer\Products\""
	aKey = winreg.OpenKey(aReg, r"SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall",
						  0, winreg.KEY_READ | win_flag)

	count_subkey = winreg.QueryInfoKey(aKey)[0]
	#way to make it so we don't infinite loop
	
	latest_version = {}
	latest_version["version"] = 0

	for i in range(count_subkey):
		software = {}
		try:
			asubkey_name = winreg.EnumKey(aKey, i)
			asubkey = winreg.OpenKey(aKey, asubkey_name)
			name = winreg.QueryValueEx(asubkey, "DisplayName")[0]
			if software_name.lower() in name.lower():
				software['name'] = name
				try:
					software['version'] = winreg.QueryValueEx(asubkey, "DisplayVersion")[0]
				except EnvironmentError:
					software['version'] = 'undefined'
				try:
					software['publisher'] = winreg.QueryValueEx(asubkey, "Publisher")[0]
				except EnvironmentError:
					software['publisher'] = 'undefined'
				try:
					software['path'] = "{}".format(winreg.QueryValueEx(asubkey, "InstallLocation")[0])
				except EnvironmentError:
					software['path'] = 'undefined'

				if software["version"] > str(latest_version["version"]) or latest_version["version"] == 0:
					latest_version = software
				
		except EnvironmentError:
			continue
	#search the other flag
	if search_64_first:
		search_64_first = False
	else:
		search_64_first = True

	if first_call and latest_version["version"] == 0:
		latest_version = latest_installed_program(software_name, search_64_first, False)

	return latest_version


latest_installed_program("Autodesk Maya")
latest_installed_program("3ds Max")
latest_installed_program("Photoshop")


"""
returns
#3ds Max {'version': '20.4.0.4254', 'publisher': 'Autodesk', 'path': 'C:\\Program Files\\Autodesk\\3ds Max 2018\\', 'name': 'Autodesk 3ds Max 2018'}
Autodesk Maya {'version': '18.0.0.5870', 'publisher': 'Autodesk', 'path': 'D:\\Program Files\\Autodesk\\Maya2018\\', 'name': 'Autodesk Maya 2018'}
Photoshop {'version': '20.0.1', 'publisher': 'Adobe Systems Incorporated', 'path': 'C:\\Program Files\\Adobe\\Adobe Photoshop CC 2019', 'name': 'Adobe Photoshop CC 2019'}
"""
```
## Backlinks
* [[Python3 Snippets]]
	* [[[Python]-Py3-Find-latest-installed-program-on-system]]

