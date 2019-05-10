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
