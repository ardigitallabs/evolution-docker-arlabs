# evolution-docker-arlabs

### Passo a Passo: Instalação do Docker no Ubuntu Server

Este guia detalha como instalar o Docker no **Ubuntu Server** usando o repositório oficial do Docker e como iniciar o projeto **Evolution** utilizando o `docker-compose.yml` configurado.

---

### **1. Atualizar o sistema**
Primeiro, atualize os pacotes do sistema para garantir que tudo esteja atualizado:

```bash
sudo apt-get update
sudo apt-get upgrade -y
```

---

### **2. Configurar o repositório do Docker**
Antes de instalar o Docker, é necessário configurar o repositório oficial.

#### **2.1. Adicionar certificados e ferramentas necessárias**
Execute o seguinte comando para instalar as dependências:

```bash
sudo apt-get install -y ca-certificates curl
```

#### **2.2. Adicionar a chave GPG oficial do Docker**
Baixe e configure a chave GPG do Docker:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

#### **2.3. Adicionar o repositório Docker no APT**
Adicione o repositório oficial à lista de fontes do sistema:

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Atualize o cache do APT:

```bash
sudo apt-get update
```

---

### **3. Instalar o Docker**
Agora que o repositório está configurado, você pode instalar os pacotes do Docker.

#### **3.1. Instalar o Docker Engine e plugins**
Instale os pacotes principais do Docker:

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

### **4. Verificar a instalação**
Teste a instalação do Docker executando um contêiner de teste:

```bash
sudo docker run hello-world
```

Se a instalação foi bem-sucedida, você verá uma mensagem de confirmação indicando que o Docker foi instalado corretamente.

---

### **5. (Opcional) Configurar o Docker para rodar sem `sudo`**
Por padrão, você precisa usar `sudo` para executar comandos Docker. Para permitir que seu usuário execute comandos sem `sudo`, siga os passos:

#### **5.1. Adicionar o usuário ao grupo `docker`**
Execute o comando abaixo substituindo `<seu_usuario>` pelo nome do seu usuário:

```bash
sudo usermod -aG docker <seu_usuario>
```

#### **5.2. Aplicar as alterações**
Para que a alteração tenha efeito, reinicie a sessão ou execute:

```bash
newgrp docker
```

Agora, você pode usar o Docker sem `sudo`.

---

### **6. Configurações Pós-Instalação (Opcional)**
Se desejar, configure o Docker para iniciar automaticamente com o sistema:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

---

### **7. Clonar o Repositório e Configurar o Projeto Evolution**

#### **7.1. Clonar este repositório**
No servidor onde o Docker foi instalado, clone o repositório do projeto:

```bash
git clone https://github.com/ardigitallabs/evolution-docker-arlabs.git
cd evolution-docker-arlabs
```

#### **7.2. Configurar as pastas necessárias**
Certifique-se de que a estrutura de pastas está configurada corretamente. O arquivo `docker-compose.yml` automaticamente cria a pasta `data` para armazenar os dados do PostgreSQL. Certifique-se de que os arquivos estão organizados assim:

```plaintext
.
├── docker-compose.yml
├── config
│   ├── postgresql.conf
│   ├── pg_hba.conf
├── data/  # Criada automaticamente pelo Docker
└── README.md
```

---

### **8. Iniciar o Projeto Evolution**

#### **8.1. Configurar o ambiente**
Certifique-se de que o arquivo `.env` está configurado com as variáveis de ambiente necessárias para o projeto.

#### **8.2. Subir os contêineres**
Execute o seguinte comando para iniciar o projeto:

```bash
docker compose up -d
```

Este comando iniciará todos os serviços definidos no `docker-compose.yml` (PostgreSQL, Redis e Evolution API) em segundo plano.

#### **8.3. Verificar os logs**
Para garantir que todos os contêineres estejam funcionando corretamente, veja os logs:

```bash
docker compose logs -f
```

#### **8.4. Acessar o sistema**
Se tudo estiver funcionando corretamente, o sistema Evolution estará acessível na porta `8080` (ou outra configurada no `docker-compose.yml`).

Abra o navegador e acesse:

```
http://<seu_servidor>:8080
```

---

### **Resumo dos Comandos**
Aqui está uma lista consolidada dos comandos usados neste guia:

```bash
# Atualizar o sistema
sudo apt-get update
sudo apt-get upgrade -y

# Instalar dependências e configurar o repositório Docker
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Instalar Docker
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Verificar instalação
sudo docker run hello-world

# Clonar repositório e configurar o projeto
git clone https://github.com/ardigitallabs/evolution-docker-arlabs.git
cd evolution-docker-arlabs

# Subir os contêineres
sudo docker compose up -d

# Verificar logs
sudo docker compose logs -f
```

---

### **Notas Importantes**
- Este tutorial foi criado para **Ubuntu Server**, mas funciona em qualquer distribuição baseada no Ubuntu.
- Caso utilize uma distribuição derivada (como Linux Mint), substitua `$(. /etc/os-release && echo "$VERSION_CODENAME")` por `$(lsb_release -sc)` ao adicionar o repositório Docker.
- Para alterações no comportamento do PostgreSQL, edite os arquivos `config/postgresql.conf` e `config/pg_hba.conf` antes de subir os contêineres.

Pronto! Agora você possui um ambiente completo para executar o projeto Evolution com Docker no Ubuntu Server.
