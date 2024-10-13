# Linux
ls
cd
mkdir
rm
ll (ls -la)
groups
cat
/my_script
chmod u+x my_script.sh
cat etc/passw
porty
localhost 127.0.0.1
python3 -m http.server 8000 --bind 127.0.0.1 (serwer HTTP w Pythonie działa w trybie przeglądania plików)
(ss -tuln)



# 1. Instalacja

   https://docs.docker.com/engine/install/ubuntu/
   
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

```shell
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


```shell
sudo docker run hello-world
```

https://docs.docker.com/engine/install/linux-postinstall/

```shell
sudo groupadd docker
```


```shell
sudo usermod -aG docker $USER
```

```shell
newgrp docker
```

```shell
docker run hello-world
```


docker pull nginx
docker images
docker docker run nginx
docker run -p 127.0.0.1:80:80 nginx -d
docker ps 
docker stop
docker ps -a
docker images rm

# 2. Pobranie FE

   https://github.com/EventiloHub/eventilo_hub_frontend

```shell
git clone git@github.com:EventiloHub/eventilo_hub_frontend.git
```

Profile -> Settings -> SSH and GPG keys -> SSH keys

(https://www.unixtutorial.org/how-to-generate-ed25519-ssh-key/)

~/.ssh

```shell
docker compose up --build
```



# 3.  pipx

https://pipx.pypa.io/stable/installation/

```shell
sudo apt update
sudo apt install pipx
pipx ensurepath
```

# 4. Poetry

https://python-poetry.org/docs/#installation

```bash
pipx install poetry
```

```bash
pipx upgrade poetry
```

```shell
poetry init
poetry add sth
poetry env info
poetry config virtualenvs.in-project true
poetry shell
poetry remove sth
```


# 5. BE

```shell
mkdir eventilo-backend
cd eventilo-backend
poetry init
code .
```

Sample .gitignore

```
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class
# C extensions
*.so
# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST
# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec
# Installer logs
pip-log.txt
pip-delete-this-directory.txt
# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
*.py,cover
.hypothesis/
.pytest_cache/
cover/
# Translations
*.mo
*.pot
# Django stuff:
*.log
local_settings.py
db.sqlite3
db.sqlite3-journal
# Flask stuff:
instance/
.webassets-cache
# Scrapy stuff:
.scrapy
# Sphinx documentation
docs/_build/
# PyBuilder
.pybuilder/
target/
# Jupyter Notebook
.ipynb_checkpoints
# IPython
profile_default/
ipython_config.py
# pyenv
#   For a library or package, you might want to ignore these files since the code is
#   intended to run in multiple environments; otherwise, check them in:
# .python-version
# pipenv
#   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
#   However, in case of collaboration, if having platform-specific dependencies or dependencies
#   having no cross-platform support, pipenv may install dependencies that don't work, or not
#   install all needed dependencies.
#Pipfile.lock
# PEP 582; used by e.g. github.com/David-OConnor/pyflow
__pypackages__/
# Celery stuff
celerybeat-schedule
celerybeat.pid
# SageMath parsed files
*.sage.py
# Environments
.venv
env/
venv/
ENV/
env.bak/
venv.bak/
.env
# Spyder project settings
.spyderproject
.spyproject
# Rope project settings
.ropeproject
# mkdocs documentation
/site
# mypy
.mypy_cache/
.dmypy.json
dmypy.json
# Pyre type checker
.pyre/
# pytype static type analyzer
.pytype/
# Cython debug symbols
cython_debug/
# Text Editor
.vscode
# macOS automatic files
.DS_Store
```

```shell
git add .
git commit -m "Initial commit"

git remote add origin git@github.com:Eventilo/eventilo-backend.git

git push origin main
```

fastapi hello world

app/main.py

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
return {"message": "Hello World!"}
```

Dockerfile
```Dockerfile
FROM python:3.12-slim
WORKDIR /code
RUN pip install --no-cache-dir poetry
COPY pyproject.toml poetry.lock* ./
RUN poetry config virtualenvs.create false
RUN poetry install
COPY . .
```

docker-compose.yml
```yml
  api:
    build: .
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
    ports:
      - "8000:8000"
```

poetry add fastapi and uvicorn

```shell
docker compose up --build
docker ps
docker exec --it ... bash
```


**`.:/code`** (Bind Mount): Mapowanie lokalnego katalogu z kodem na katalog w kontenerze. Służy głównie do pracy deweloperskiej, aby natychmiast wprowadzać zmiany w kontenerze.

add 
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
    volumes:
      - .:/code



```yaml
services:
  db:
    image: postgres:17
    environment:
      POSTGRES_USER: eventilo
      POSTGRES_PASSWORD: eventilo
      POSTGRES_DB: eventilo

  api:
    build: .
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```

```shell
docker compose up --build
docker ps
docker exec -it ... bash
psql -U eventilo
```

```sql
CREATE TABLE cars (  
  brand VARCHAR(255),  
  model VARCHAR(255),  
  year INT  
);
```

docker stop rm 
docker compose up --build

**`postgres_data:/var/lib/postgresql/data`** (Named Volume): Trwały wolumen zarządzany przez Dockera, służący do przechowywania danych bazy danych, które powinny przetrwać restart lub usunięcie kontenera.


```yaml
services:
  db:
    image: postgres:17
    environment:
      POSTGRES_USER: eventilo_hub
      POSTGRES_PASSWORD: eventilo_hub
      POSTGRES_DB: eventilo_hub
    volumes:
      - postgres_data:/var/lib/postgresql/data

  api:
    build: .
    command: uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db

volumes:
  postgres_data:
```

