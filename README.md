# Instalação do MongoDB no Ubuntu 22.04

Guia para instalar o MongoDB em Linux Ubuntu em 7 passos.

## Descrição
Este repositório tem como propósito de ajudar a instalar o MongoDB na versão 22.04 do Ubuntu. Muitos usuários apresentaram problemas ao instalar o MongoDB nesta versão do Ubuntu. Tentarei ajudar com os passos para a instalação

<!--## Conteúdo
- **-->

## Índice
- [x] <a href="#intro">Introdução ao MongoDB</a>
- [x] <a href="#instalacao">Instalação do MongoDB em Ubuntu 22.04</a>
- <a href="#servico">Inciando e habilitando o serviço do MongoDB</a>
- <a href="#criacao">Criando um banco de dados e usuário em MongoDB</a>
- <a href="#parametros">Ajustando parâmetros de Segurança do MongoDB em Ubuntu</a>
- <a href="#remote">Configurando MongoDB para acesso remoto</a>
- <a href="#trabalhando">Trabalhando com banco de dados MongoDB</a>


<p id="intro"><h2>Introdução ao MongoDB</h2></p>

### O que é o MongoDB?
O desenvolvimento de aplicativos escalonáveis e de alto desempenho fez uso extensivo do MongoDB, um banco <a href="https://www.pcperformance.com.br/glossario/o-que-e-cross-platform/">cross-plataform</a> e de código aberto. Em termos de modelagem e organização de dados, o MongoDB é diferente dos bancos de dados SQL convencionais.

Em vez de relações tabulares rígidas, o MongoDB é construído sobre um modelo de dados flexível e orientado a documentos. Cada entidade é armazenada como um documento BSON – uma representação binária de pares de valores de campo semelhantes a JSON. Os documentos contêm estruturas hierárquicas com matrizes e subdocumentos, espelhando dados complexos do mundo real.

Algumas caracteristicas do MongoDB são: modelo de documento declarativo, os esquemas dinâmicos e recursos de escalabilidade nativa. Isso torna-o um banco muito amigável com o desenvolvimento ágil por não ter muitas restrições (schemaless).

<p id="instalacao"><h2>Passo 1: Instale o MongoDB no Ubuntu 22.04</h1> </p>

Depois de concluir estas etapas a seguir, você poderá instalar e configurar o MongoDB em seu sistema Ubuntu 22.04. As duas primeiras etapas orientam você no processo de instalação. As etapas restantes detalham como criar um banco de dados e usuários do banco de dados, proteger o servidor de banco de dados, configurar o acesso remoto e trabalhar com o banco de dados MongoDB.

### 1) A primeira etapa é instalar os pacotes de pré-requisitos necessários durante a instalação. Para fazer isso, execute o seguinte comando:
```
sudo apt install software-properties-common gnupg apt-transport-https ca-certificates -y
```
#### 1.1) Caso apareça este erro:

![image](https://github.com/martinsRossi/mongodb_ubuntu_22.04/assets/101609697/b49b2c9b-7397-4ef9-9576-b464f04bf6f7)

Execute:

```
su
```
Coloque sua senha. Após isso, digite:

```
sudo nano /etc/sudoers
```
Logo abaixo de %sudo ALL =(ALL:ALL) ALL, acrescente:

```
vboxuser ALL =(ALL:ALL) ALL
```
Ctrl + O (Salvar) - Enter
Ctrl + X (Fechar)
Ctrl + D (Sair modo root)

### 2) Para instalar o pacote MongoDB mais recente, você precisa adicionar o repositório do pacote MongoDB ao arquivo de lista de fontes no Ubuntu. Antes disso, você precisa importar a chave pública do MongoDB em seu sistema usando o comando wget da seguinte forma:

```
curl -fsSL https://pgp.mongodb.com/server-7.0.asc |  sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
```
#### 2.1) Caso o curl não esteja instalado, rode:

```
sudo apt update
sudo apt upgrade
```
#### Por fim, rode:

```
sudo apt install curl
```
### 3) Em seguida, adicione o repositório MongoDB 7.0 APT ao diretório /etc/apt/sources.list.d.

```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```
![image](https://github.com/martinsRossi/instalacao-mongodb/assets/101609697/9475c3a4-3966-4660-bd33-282a92bae5da)

### 4) Depois que o repositório for adicionado, recarregue o índice do pacote local.

```
sudo apt update
```
#### O comando atualiza os repositórios locais e informa o Ubuntu sobre o repositório MongoDB 7.0 recém-adicionado.

### 5) Com isso resolvido, instale o meta-package <code>mongodb-org</code> que fornece o MongoDB.

```
sudo apt install mongodb-org -y
```
![image](https://github.com/martinsRossi/instalacao-mongodb/assets/101609697/9571a1d7-56e7-41e7-bebd-e522c82fdae1)

### 6) O comando instala o servidor de banco de dados MongoDB junto com os componentes principais do banco de dados, incluindo as ferramentas shell. Assim que a instalação for concluída, verifique a versão do MongoDB instalada:

```
 mongod --version
```
![image](https://github.com/martinsRossi/instalacao-mongodb/assets/101609697/9ef7d599-aa97-4428-ada3-27c2e62b3ede)

Isso lista um monte de informações, incluindo a versão, Git e versão OPenSSL, entre outros detalhes.

<p id="servico"><h2>Passo 2: Inciando e habilitando o serviço do MongoDB</h2></p>

### 1) O serviço MongoDB é desabilitado na instalação por padrão e você pode verificar isso executando o comando abaixo:
```
sudo systemctl status mongod
```
![image](https://github.com/martinsRossi/mongodb-ubuntu-22.04/assets/101609697/ef8ea699-a182-49dc-a5bb-33c700ce5e4b)

### 2) Para iniciar o serviço MongoDB, execute o comando:
```
sudo systemctl start mongod
```

### 3) Mais uma vez, confirme se o serviço está em execução:
```
sudo systemctl status mongod
```
![image](https://github.com/martinsRossi/mongodb-ubuntu-22.04/assets/101609697/3c1aa593-e19b-467f-aa9c-e3de1f6ae5b4)

### 4) Na saída acima, você pode ver que o MongoDB está instalado e funcionando. Além disso, você pode confirmar se o banco de dados está funcionando verificando se o servidor está escutando em sua porta padrão, que é a porta 27017.
### Para fazer isso, execute o seguinte comando <code>ss</code>:
```
$ sudo ss -pnltu | grep 27017
```
### Você deverá ver a seguinte saída em seu terminal.

![image](https://github.com/martinsRossi/mongodb-ubuntu-22.04/assets/101609697/ca8545ea-9735-4941-80cc-cafe863b5ec3)

### 5) Depois de verificar se o serviço está funcionando conforme esperado, agora você pode ativar o MongoDB para iniciar na inicialização conforme mostrado.
```
sudo systemctl enable mongod
```
![image](https://github.com/martinsRossi/mongodb-ubuntu-22.04/assets/101609697/693d44b1-33e1-4471-9027-53f34fa8287f)
### Até agora, o MongoDB foi instalado e configurado com sucesso para iniciar na inicialização.

<p id="criacao"><h2>Passo 3: Criando um banco de dados em MongoDB</h2></p>

### 1) Para iniciar o banco, digite:

```
mongosh
```

### 2) Para verificar os bancos de dados já existentes, digite:

```
show dbs
```

### 3) Para criar um banco, digite <code>use</code> e o nome do banco. Para nosso exemplo, iremos criar um banco de dados do Digimom.

```
use digimon_database
```

<p id="tabela"><h2>Passo 4: Criando e adicionando em tabelas no MongoDB</h2></p>

#### 1) Para criação de uma tabela, com inserção de dados, inserimos:



```
db.createCollection("pokemon")
```

### 2) Nós adicionaremos este Digimom com este comando:
```
db.pokemon.insertOne({
    "nome": "Pikachu",
    "tipo": "Elétrico",
    "habilidades": ["Choque do Trovão", "Investida"],
    "atributos": {
        "ataque": 55,
        "defesa": 40,
        "velocidade": 90
    }
})

```

### 3) Se queremos adicionar mais de um usamos o comando <code>insertMany</code>:

```
db.pokemon.insertMany([{
        "nome": "Bulbasaur",
        "tipo": "Planta",
        "habilidades": ["Chicote de Vinha", "Raio Solar"],
        "atributos": {
        "ataque": 49,
        "defesa": 49,
        "velocidade": 45
        }
    },
    {
        "nome": "Charmander",
        "tipo": "Fogo",
        "habilidades": ["Lança-Chamas", "Garra de Metal"],
        "atributos": {
            "ataque": 52,
            "defesa": 43,
            "velocidade": 65
        }
    }
])

```
### 4) Tipos de consultas

#### 4.1) Para consultarmos o primeiro que criamos, usamos:

```
db.pokemon.findOne()
```

#### 4.2) Afim de consultar todos, usamos:

```
db.pokemon.find()
```

#### 4.3) Consulta com filtro Menor que (qualquer campo):

```
db.pokemon.find({"atributos.ataque": {$lt: 50}})
```

#### 4.4) Consulta com filtro Maior que e Menor que:

```
db.pokemon.find({"atributos.defesa": {$gt: 40, $lt: 50}})
```

### 5) Se quisermos saber quantos registros foram adicionados, digitamos:

```
db.pokemon.count()
```

### 6) Consulta com contagem com algum filtro de registros na tabela:

```
db.pokemon.count({"tipo": "Fogo"})
```

<!--
<p id="database"></p>

## Criando um banco de dados e usuário em MongoDB

<p id="parametros"></p>

## Ajustando parâmetros de Segurança do MongoDB em Ubuntu

<p id="remote"></p>

## Configurando MongoDB para acesso remoto

<p id="trabalhando"></p>

## Trabalhando com banco de dados MongoDB 
-->
