# __Install AUTOMATIC1111 with the proper python version and venv on Ubuntu 24.04__

_The idea is to have a portable version and not change anything on your system_

---

## Create and enter in to directory

```bash
mkdir AUTOMATIC1111
cd AUTOMATIC1111
```

## Download and compile Python-3.10.6

### Install dependencies

```bash
apt install wget git build-essential libreadline-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev
```

### Download and extract Python-3.10.6

```bash
wget https://www.python.org/ftp/python/3.10.6/Python-3.10.6.tar.xz
tar -xvf Python-3.10.6.tar.xz
rm Python-3.10.6.tar.xz
```

### Compile Python-3.10.6

```bash
cd Python-3.10.6/
./configure
make -j $(nproc) # or just make
./python -c "import sqlite3; print(sqlite3.sqlite_version)" # test if library is working
cd ..
```

## Create python virtual environment

```bash
Python-3.10.6/python -m venv venv-3.10.6
source venv-3.10.6/bin/activate
```

## Install AUTOMATIC1111

```bash
# Download
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui
# Start
stable-diffusion-webui/webui.sh
```
### Settings (optional)

Change `export COMMANDLINE_ARGS` in `stable-diffusion-webui/webui-user.sh`

- For less then 4GB VRAM set `export COMMANDLINE_ARGS="--lowvram"`
- For 4GB-6GB VRAM set `export COMMANDLINE_ARGS="--medvram"`
- For more then 6GB leave it commented

## Create run script (optional)

```bash
echo "source venv-3.10.6/bin/activate && stable-diffusion-webui/webui.sh" > run
chmod +x run
./run
```


