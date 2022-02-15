# Format yaml 

```
# Kommentare 

# Listen 
- rot
- gruen
- blau 

# Mappings 
Version: 3.7 

# Mappings können auch Listen enthalten 
expose: 
  - "3000"
  - "8000" 

# Verschachtelte Mappings 
build:
  context: .
  labels: 
    label1: "bunt"
    label2: "hell" 

```
