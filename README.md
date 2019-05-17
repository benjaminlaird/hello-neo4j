# hello-neo4j

Download Neo4j desktop package: https://neo4j.com/download-thanks-desktop/?edition=desktop&flavour=osx&release=1.1.22&offline=true

# Add first node

```
$ CREATE (a:Company {Name: "Slack Technologies"})
```

![image](https://user-images.githubusercontent.com/2372344/57538390-512eb100-7316-11e9-890d-1f8e06bb6dab.png)

# Visualize the node
```
$ match (c:Company) return c
```
![image](https://user-images.githubusercontent.com/2372344/57540048-c6e84c00-7319-11e9-894e-a18b5f12f4f3.png)

# Delete duplicate company nodes without relationships
```
match (n:Company {Name: "Slack Technologies"}) Delete n
```

# Add `Person`->`Company` relationship
```
MATCH (c {name: "Slack Technologies"}) CREATE (p:Person {name:"Cal Henderson" }) - [rel:WORKS_AT {role:"CTO"}] -> (c)
```

# Add `role` to the Person->Company relationship

```
$ MATCH (a {name: "Stewart Butterfield"}) - [r] -> (c {name: "Slack Technologies"}) set r.role = "CEO"
```

![image](https://user-images.githubusercontent.com/2372344/57548693-9c08f280-732f-11e9-837c-c3d6c19da33e.png)

# Build out VC relationships
```
CREATE (p:Person {name:"Andrew Braccia" }) - [rel:WORKS_AT {role:"Partner"}] -> (c:Company {name:"Accel"})
MATCH (c {name: "Slack Technologies"}), (p {name:"Andrew Braccia"}) CREATE (p) - [rel:INVESTED_IN {round:"Series A", amount: "$5M"}] -> (c)

MATCH (c {name: "Slack Technologies"}) - [r*1..3] - (p) return r,c,p
```
![image](https://user-images.githubusercontent.com/2372344/57549947-e2ac1c00-7332-11e9-9c32-bd17979ff251.png)


#### Python Querying

```
from neo4j import GraphDatabase
driver = GraphDatabase.driver("bolt://localhost:7687", auth=("neo4j", "XXX"))
def print_company(tx, name):
    for record in tx.run("MATCH (c {name: $company})-[r*1..3]-(p) return p", company=name):
        print(record)
   

with driver.session() as session:
    session.read_transaction(print_company, "Slack Technologies")


<Record p=<Node id=40 labels={'Person'} properties={'name': 'Stewart Butterfield'}>>
<Record p=<Node id=0 labels={'Person'} properties={'name': 'Cal Henderson'}>>
<Record p=<Node id=78 labels={'Person'} properties={'name': 'Allen Shim'}>>
<Record p=<Node id=79 labels={'Person'} properties={'name': 'Tamar Yehoshua'}>>
<Record p=<Node id=80 labels={'Person'} properties={'name': 'Terry Anderson'}>>
<Record p=<Node id=42 labels={'Person'} properties={'name': 'Robert Frati'}>>
<Record p=<Node id=21 labels={'Person'} properties={'name': 'Robby Kwok'}>>
<Record p=<Node id=20 labels={'Person'} properties={'name': 'Allan Leinwand'}>>
<Record p=<Node id=22 labels={'Person'} properties={'name': 'David Schellhase'}>>
<Record p=<Node id=23 labels={'Person'} properties={'name': 'Geoff Belknap'}>>
<Record p=<Node id=60 labels={'Person'} properties={'name': 'Andrew Braccia'}>>
<Record p=<Node id=61 labels={'Company'} properties={'name': 'Accel'}>>
```
