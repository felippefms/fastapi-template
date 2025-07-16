<p align="center">
  <a href="https://fastapi.tiangolo.com"><img src="https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png" alt="FastAPI"></a>
</p>

# fastapi-template

Template para iniciar projetos com Fastapi

## Requisitos
    Python -> download no site
    pipx -> pip install pipx
    ensure path do pipx -> pipx ensurepath
    Poetry -> pipx install poetry
    Poetry Shell -> pipx inject poetry poetry-plugin-shell

## Iniciar um novo projeto

Na pasta base dos projetos (ex: VsCode) executar:
```bash
poetry new --flat fastapi-template
```
(api ou o nome do projeto que quiser dar, se tiver espaços use - e não _ )

## Defina a versão minima e máxima do python para o projeto (evita erro de instalação de algumas libs)

No arquivo pyproject.toml mude a linha:
```bash
requires-python = ">=3.13,<4.0"
```
Exemplo: O python está na versão **3.x**, então usaremos ela como base e no máximo usaremos MENOR QUE o próximo major update ex: **4.0**

## Poetry Install
Execute o comando na pasta que possui o pyproject.toml:
```bash
poetry install
```

## Inicie o projeto Fastapi
```bash
poetry add 'fastapi[standard]'
```

## Inicie o ambiente virtual (venv)
```bash
poetry shell
```

## Crie o arquivo base do projeto
Crie o arquivo base da fastapi dentro da pasta do projeto aonde tem o __init__.py 

(pode ter qualquer nome: **app.py**, **api.py**, **main.py**)

## Adicione alguma função mínima
Dentro do arquivo coloque a importação da fastapi com alguma rota minima 

**⚠️ (caso haja algum warning de import mude o interptretador do python)**

```python
from fastapi import FastAPI 

app = FastAPI()

@app.get('/')
def read_root():
    return {'message': 'Olá Mundo!'}
```

## Testar a aplicação (Opcional)
**⚠️ (verifique se você salvou o conteúdo do arquivo **app.py** depois de colar)**

**⚠️ (verifique se você está dentro do venv do poetry shell)**

```bash
fastapi dev fastapi_template/app.py
```

## Instale as libs adicionais (recomendado)
### Lista de libs adicionais
- **taskipy**: ferramenta para criar comandos. Como executar a aplicação, rodar testes, etc.
- **pytest**: ferramenta para escrever e executar testes
- **ruff**: linter e formatador de código para seguir a [PEP-8](https://peps.python.org/pep-0008/)🔗

⚠️ **--group dev** irá instalar como dev dependency

```bash
poetry add --group dev pytest pytest-cov taskipy ruff
```

## Configure as libs instaladas
altere o arquivo **pyproject.toml**

⚠️ **Atenção nas partes que precisam modificar o nome do projeto, o nome precisa ser alterado para o nome da pasta aonde existe seu arquivo principal do Fastapi, nesse caso procure as linhas com nome "fastapi_template" nos comandos run e test**

```toml
[tool.ruff]
line-length = 79
extend-exclude = ['migrations']

[tool.ruff.lint]
preview = true
select = ['I', 'F', 'E', 'W', 'PL', 'PT']

[tool.ruff.format]
preview = true
quote-style = 'single'

[tool.pytest.ini_options]
pythonpath = "."
addopts = '-p no:warnings'

[tool.taskipy.tasks]
lint = 'ruff check'
pre_format = 'ruff check --fix'
format = 'ruff format'
run = 'fastapi dev fastapi_template/app.py'
pre_test = 'task lint'
test = 'pytest -s -x --cov=fastapi_template -vv'
post_test = 'coverage html'
```

## Crie um gitignore
Utilize um gitignore específico para python

```bash
pipx run ignr -p python > .gitignore
```

## Utilizando a aplicação:

- Iniciando a aplicação:
```bash
poetry run task run
```

- Rodando testes:
```bash
poetry run task test
```

⚠️  A run do **task_test** ou do **task_format** já realizam os seus comandos **pré-test** e **post_test** ou **pré-format**.