# Racktables API
29-04-2025
Tags: #services 


```bash
sudo apt install python3.10-venv
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```
- requirements.txt
```
mysql-connector-python
```
- Chạy hai file check để xem liệu có vấn đề gì không tương thích với db hay không (thay đổi đúng user, password để có thể log vào db)
```
nano check.py
```

```python
import mysql.connector
import sys

# Define required tables and their columns
required_structure = {
    'Attribute': ['id', 'name'],
    'Dictionary': ['dict_key', 'dict_value'],
    'Object': ['id', 'name', 'label', 'objtype_id', 'asset_no', 'has_problems', 'comment'],
    'AttributeValue': ['object_id', 'object_tid', 'attr_id', 'string_value', 'uint_value', 'float_value'],
    'IPv4Allocation': ['object_id', 'ip', 'name', 'type'],
    'TagTree': ['id', 'tag'],
    'TagStorage': ['entity_realm', 'entity_id', 'tag_id', 'tag_is_assignable', 'user', 'date']
}

# Connect to the database
try:
    db = mysql.connector.connect(
        host="localhost",
        user="racktables",
        password="", #NHẬP PASS Ở ĐÂY
        database="racktables"
    )
    cursor = db.cursor()
    print("Connected to database.")
except mysql.connector.Error as e:
    print(f"Error connecting to database: {e}")
    sys.exit(1)

# Function to fetch all existing tables
def get_existing_tables():
    cursor.execute("SHOW TABLES")
    tables = [row[0] for row in cursor.fetchall()]
    return tables

# Function to fetch all columns for a table
def get_columns(table):
    cursor.execute(f"DESCRIBE {table}")
    columns = [row[0] for row in cursor.fetchall()]
    return columns

# Start checking
existing_tables = get_existing_tables()
missing_tables = []
missing_columns = {}

for table, expected_columns in required_structure.items():
    if table not in existing_tables:
        missing_tables.append(table)
    else:
        existing_columns = get_columns(table)
        missing = [col for col in expected_columns if col not in existing_columns]
        if missing:
            missing_columns[table] = missing

# Reporting
if missing_tables:
    print("Missing tables:")
    for table in missing_tables:
        print(f" - {table}")
if missing_columns:
    print("Missing columns:")
    for table, columns in missing_columns.items():
        print(f" - {table}: missing {', '.join(columns)}")

if not missing_tables and not missing_columns:
    print("✅ Database structure OK.")
else:
    print("\n❌ Database structure mismatch detected.")
    sys.exit(1)

# Clean up
cursor.close()
db.close()
```

```bash
python3 check.py
```

Output:

![[Ảnh màn hình 2025-04-29 lúc 10.22.57.png]]

- File check thứ 2. Cần rename file csv thành page5.csv
```bash
nano check2.py
```

```python
import mysql.connector
import csv
import sys

# Fields to ignore when validating attributes
ignore_fields = {'STT', 'Server', 'Type', 'ens160', 'ens192', 'Tags', 'OS version'}

# Connect to database
try:
    db = mysql.connector.connect(
        host="localhost",
        user="racktables",
        password="StrongPassword",  # ĐỔI PASS Ở ĐÂY
        database="racktables"
    )
    cursor = db.cursor()
    print("Connected to database.")
except mysql.connector.Error as e:
    print(f"Error connecting to database: {e}")
    sys.exit(1)

# 1. Check if tables and columns exist (basic structure check)
required_structure = {
    'Attribute': ['id', 'name'],
    'Dictionary': ['dict_key', 'dict_value'],
    'Object': ['id', 'name', 'label', 'objtype_id', 'asset_no', 'has_problems', 'comment'],
    'AttributeValue': ['object_id', 'object_tid', 'attr_id', 'string_value', 'uint_value', 'float_value'],
    'IPv4Allocation': ['object_id', 'ip', 'name', 'type'],
    'TagTree': ['id', 'tag'],
    'TagStorage': ['entity_realm', 'entity_id', 'tag_id', 'tag_is_assignable', 'user', 'date']
}

def get_existing_tables():
    cursor.execute("SHOW TABLES")
    return [row[0] for row in cursor.fetchall()]

def get_columns(table):
    cursor.execute(f"DESCRIBE {table}")
    return [row[0] for row in cursor.fetchall()]

existing_tables = get_existing_tables()
missing_tables = []
missing_columns = {}

for table, expected_columns in required_structure.items():
    if table not in existing_tables:
        missing_tables.append(table)
    else:
        existing_columns = get_columns(table)
        missing = [col for col in expected_columns if col not in existing_columns]
        if missing:
            missing_columns[table] = missing

if missing_tables or missing_columns:
    print("\n❌ Basic database structure problem detected:")
    if missing_tables:
        print("Missing tables:", missing_tables)
    if missing_columns:
        for table, cols in missing_columns.items():
            print(f"Missing columns in {table}: {cols}")
    sys.exit(1)
else:
    print("✅ Basic table structure check passed.")

# 2. Read header from CSV to get dynamic attribute fields
csv_file = 'page5.csv'

try:
    with open(csv_file, 'r') as file:
        reader = csv.reader(file)
        headers = next(reader)
except FileNotFoundError:
    print(f"\n❌ CSV file '{csv_file}' not found.")
    sys.exit(1)

# Dynamic attributes to check (everything except ignored fields)
attributes_to_check = [header.strip() for header in headers if header.strip() not in ignore_fields]

# 3. Load attribute map
cursor.execute("SELECT id, name FROM Attribute")
attribute_map = {name: id for id, name in cursor.fetchall()}

# Check required attributes dynamically
missing_attributes = [attr for attr in attributes_to_check if attr not in attribute_map]
if missing_attributes:
    print("\n❌ Missing required attributes:", missing_attributes)
    sys.exit(1)
else:
    print("✅ All dynamic attributes from CSV found in Attribute table.")

# 4. Check type (example 'VM') exists in Dictionary
cursor.execute("SELECT dict_key FROM Dictionary WHERE dict_value = %s", ('VM',))
type_row = cursor.fetchone()
if not type_row:
    print("\n❌ Required VM type 'VM' not found in Dictionary table.")
    sys.exit(1)
else:
    print(f"✅ VM type 'VM' found with dict_key {type_row[0]}.")

# 5. Check Tags and OS Versions existence
tags_in_csv = set()
os_versions_in_csv = set()
servers_in_csv = []

with open(csv_file, 'r') as file:
    reader = csv.DictReader(file)
    for row in reader:
        servers_in_csv.append(row['Server'].strip())
        tags_in_csv.add(row['Tags'].strip())
        os_versions_in_csv.add(row['OS version'].strip())

# Check Tags
missing_tags = []
for tag in tags_in_csv:
    if tag:
        cursor.execute("SELECT id FROM TagTree WHERE tag = %s", (tag,))
        if not cursor.fetchone():
            missing_tags.append(tag)

if missing_tags:
    print("\n❌ Missing Tags in database:", missing_tags)
    sys.exit(1)
else:
    print("✅ All Tags from CSV found in TagTree.")

# Check OS Versions
missing_os_versions = []
for os_version in os_versions_in_csv:
    if os_version:
        cursor.execute("SELECT dict_key FROM Dictionary WHERE dict_value LIKE %s", (f"%{os_version}%",))
        if not cursor.fetchone():
            missing_os_versions.append(os_version)

if missing_os_versions:
    print("\n❌ Missing OS Versions in database:", missing_os_versions)
    sys.exit(1)
else:
    print("✅ All OS Versions from CSV found in Dictionary.")

# 6. Check for duplicate server names in Object table
cursor.execute("SELECT name FROM Object")
existing_servers = {row[0] for row in cursor.fetchall()}

duplicate_servers = [server for server in servers_in_csv if server in existing_servers]

if duplicate_servers:
    print("\n❌ The following servers already exist in the database:")
    for name in duplicate_servers:
        print(f" - {name}")
    sys.exit(1)
else:
    print("✅ No duplicate server names found in Object table.")

# Final success
print("\n✅✅✅ FULL DATABASE AND CSV VALIDATION PASSED ✅✅✅")

# Clean up
cursor.close()
db.close()
```

Output:

![[Ảnh màn hình 2025-04-29 lúc 10.27.11.png]]