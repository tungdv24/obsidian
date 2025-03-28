# vSphere Autodeploy DC
28-03-2025
Tags: #services 

## How to use

- Change username password in vsphere_credentials.json
- Change VM Templates in index.html line 53 
```html
<div class="mb-3">

	<label for="SourceVM" class="form-label">Source VM</label>

	<select class="form-select" id="SourceVM" name="SourceVM" required>

		<option value="Z-Template-Ubuntu22">Z-Template-Ubuntu22</option>

		<option value="Z-Template-Cent-9">Z-Template-Cent-9</option>

		<option value="tungdv-Pool-Temp">tungdv-Pool-Temp</option>

</select>å
```
- For Resource Pool, if nothing specified, it will get the resource pool of the template VMs
- Edit login users in users.json
- Command to install and run
```console
git clone https://github.kdata.vn/tungdv/autodeploy-vspheredc-2.git
apt install python3
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python3 app.py
```
