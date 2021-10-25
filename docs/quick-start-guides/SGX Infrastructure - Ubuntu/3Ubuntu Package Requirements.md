# Ubuntu Package Requirements 

Access required for the postgresql repo in all systems, through below steps:

**Create the file repository configuration**
```
sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

**Import the repository signing key**
```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

**Update the package lists**
```
apt-get update
```
