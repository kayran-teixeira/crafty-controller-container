Claro! Segue um README.md mais organizado e profissional, baseado no seu `docker-compose.yml` e seguindo o estilo que você mostrou:

# 🎮 Guia de Instalação e Execução: Crafty Controller 4 com Docker

Este guia contém as instruções necessárias para instalar e executar o **Crafty Controller 4** utilizando Docker. O Crafty é um painel web completo para gerenciamento de servidores Minecraft Java e Bedrock, oferecendo criação, monitoramento, backups e administração centralizada dos seus servidores.

---

# 📋 Requisitos

Antes de iniciar, certifique-se de possuir os seguintes componentes instalados:

* Docker
* Docker Compose (ou Docker Compose Plugin)

Verifique a instalação executando:

```bash
docker --version
docker compose version
```

---

# 📁 Estrutura do Projeto

Crie uma pasta para armazenar o Crafty Controller:

```bash
mkdir crafty
cd crafty
```

Dentro dela, crie o arquivo:

```text
docker-compose.yml
```

---

# 🛠️ Configuração do Docker Compose

Crie o arquivo `docker-compose.yml` com o conteúdo abaixo:

```yaml
services:
  crafty:
    container_name: crafty_container
    image: registry.gitlab.com/crafty-controller/crafty-4:latest
    restart: always

    environment:
      - TZ=America/Sao_Paulo

    ports:
      - "8000:8000"
      - "8443:8443"
      - "8123:8123"
      - "19132:19132/udp"
      - "25500-25600:25500-25600"

    volumes:
      - ./crafty_data/backups:/crafty/backups
      - ./crafty_data/logs:/crafty/logs
      - ./crafty_data/servers:/crafty/servers
      - ./crafty_data/config:/crafty/app/config
      - ./crafty_data/import:/crafty/app/import
```

---

# 🚀 Inicializando o Crafty

Após criar o arquivo, execute:

```bash
docker compose up -d
```

O Docker fará o download da imagem e iniciará o painel em segundo plano.

Verifique se o container está em execução:

```bash
docker ps
```

---

# 🔑 Obtendo as Credenciais Iniciais

Na primeira inicialização, o Crafty gera automaticamente um usuário administrador e uma senha temporária.

Para visualizar as credenciais:

```bash
docker logs crafty_container
```

Procure por mensagens semelhantes a:

```text
Username: admin
Password: ********
```

Guarde essas informações para o primeiro acesso.

---

# 🌐 Acessando o Painel Web

Após a inicialização, abra seu navegador e acesse:

```text
https://IP_DO_SERVIDOR:8443
```

Ou, se estiver executando localmente:

```text
https://localhost:8443
```

## Observações

* O certificado SSL é autoassinado.
* O navegador exibirá um aviso de segurança.
* Clique em **Avançado** → **Prosseguir para o site**.

Faça login utilizando as credenciais obtidas anteriormente.

---

# 🔌 Portas Utilizadas

| Porta       | Protocolo | Finalidade                |
| ----------- | --------- | ------------------------- |
| 8000        | TCP       | Interface Web HTTP        |
| 8443        | TCP       | Interface Web HTTPS       |
| 8123        | TCP       | API e serviços internos   |
| 19132       | UDP       | Minecraft Bedrock         |
| 25500-25600 | TCP       | Servidores Minecraft Java |

Caso utilize firewall ou serviços em nuvem, certifique-se de liberar as portas necessárias.

---

# ☕ Suporte ao Minecraft Java

A imagem oficial do Crafty já inclui múltiplas versões do Java, dispensando instalação manual no host.

| Versão Minecraft   | Java Recomendado |
| ------------------ | ---------------- |
| Até 1.16.4         | Java 8           |
| 1.16.5             | Java 11          |
| 1.17 até 1.20.4    | Java 17          |
| 1.20.5 ou superior | Java 21          |

O painel normalmente detecta e utiliza a versão correta automaticamente.

---

# 🧱 Suporte ao Minecraft Bedrock

Para servidores Bedrock:

* Nenhuma dependência adicional é necessária.
* O próprio Crafty realiza o download dos arquivos necessários.
* Certifique-se de liberar a porta UDP 19132 para acesso externo.

---

# 💾 Persistência de Dados

Todos os dados do painel ficam armazenados localmente na pasta:

```text
./crafty_data
```

Estrutura:

```text
crafty_data/
├── backups/
├── logs/
├── servers/
├── config/
└── import/
```

Isso garante que:

* Servidores Minecraft sejam preservados.
* Configurações permaneçam após reinicializações.
* Backups sejam mantidos.
* Atualizações não removam seus dados.

---

# 🔄 Atualizando o Crafty

Para atualizar para a versão mais recente:

```bash
docker compose pull
docker compose up -d
```

O Docker baixará a nova imagem e recriará o container mantendo todos os dados persistidos.

---

# ⏹️ Parando o Serviço

Para interromper o Crafty:

```bash
docker compose down
```

Os dados armazenados em `crafty_data` permanecerão intactos.

---

# 📦 Backup

Para realizar backup completo do ambiente:

```bash
tar -czvf crafty_backup.tar.gz crafty_data/
```

Ou simplesmente copie a pasta:

```text
crafty_data/
```

para outro local seguro.

---
