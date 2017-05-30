> requires [dockerinstallation](https://github.com/metacurrency/holochain/wiki/Docker-Installation-for-Developers)

### Install holochain binaries into your user
```bash
git clone https://github.com/metacurrency/holochain.git holochain
holochain/bin/holochain.system.install
source ~/.bashrc
```

### Work with Holochain App repository
* from the root of the App repository
```bash
holochain.app.init
git add -A
git commit -m "added holochain repo files to git repo"
```

### Run integration testing on Holochain App
* from anywhere inside the App repository
```bash
holochain.app.testScenario [tab tab]
```