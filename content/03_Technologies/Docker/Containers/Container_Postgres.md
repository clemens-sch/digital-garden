#Technologies 

---
## # Manual to install Container
### # Get Image

Open Docker

From [hub.docker.com/postgres](https://hub.docker.com/_/postgres)

```cmd
// pull Image
docker pull postgres

// Run DB Container
docker run --name postgres_schmid -e POSTGRES_PASSWORD=root1234 -e POSTGRES_USER=SCHMID -e POSTGRES_DB=persondb -p 5432:5432 -d postgres
```

