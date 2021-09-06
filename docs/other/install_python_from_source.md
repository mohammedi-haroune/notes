Inspired from: https://gist.github.com/jerblack/798718c1910ccdd4ede92481229043be
```bash
sudo apt install build-essential libssl-dev zlib1g-dev libncurses5-dev libncursesw5-dev libreadline-dev libsqlite3-dev libgdbm-dev libdb5.3-dev libbz2-dev libexpat1-dev liblzma-dev tk-dev libffi-dev
export PYTHON_VERSION=3.7.5
wget https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tar.xz
tar xvf Python-$PYTHON_VERSION.tar.xz
cd Python-$PYTHON_VERSION
./configure --enable-optimizations
# 'make -j <x>' enables parallel execution of <x> make recipes simultaneously
# Warning: I skipped it because it took too long
sudo make -j 8
# altinstall does not alter original system python install
sudo make altinstall
```
