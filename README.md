REST API para um sistema de gerenciamento de biblioteca desenvolvida em Java com Javalin

Nomes:
- Tom Alexander
- Matheus Honorato

---

## Índice

- [Stack](#stack)
- [Pré-requisitos](#pré-requisitos)
- [Configuração do ambiente](#configuração-do-ambiente)
  - [Windows — instalação do WSL](#windows--instalação-do-wsl)
  - [Instalação do JDK 25](#instalação-do-jdk-25)
  - [Instalação do Git](#instalação-do-git)
  - [Instalação do Docker Desktop](#instalação-do-docker-desktop)
  - [Instalação do IntelliJ IDEA Community](#instalação-do-intellij-idea-community)
  - [Instalação do DBeaver](#instalação-do-dbeaver)
  - [Instalação do Postman](#instalação-do-postman)
  - [Configurando o terminal WSL no IntelliJ](#configurando-o-terminal-wsl-no-intellij)
- [Clonando o repositório](#clonando-o-repositório)
- [Executando o projeto](#executando-o-projeto)
- [Fluxo de trabalho com Git](#fluxo-de-trabalho-com-git)
- [Abrindo um Pull Request](#abrindo-um-pull-request)
- [Checklist antes de abrir um PR](#checklist-antes-de-abrir-um-pr)
- [Convenção de commits](#convenção-de-commits)
- [Quadro de tarefas — como usar](#quadro-de-tarefas--como-usar)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Regras de convivência no repositório](#regras-de-convivência-no-repositório)
- [O que fazer quando travar](#o-que-fazer-quando-travar)
- [Glossário](#glossário)
- [Dúvidas frequentes](#dúvidas-frequentes)

---

## Stack

| Camada                       | Tecnologia              | Versão |
|------------------------------|-------------------------|--------|
| Linguagem                    | Java                    | 25     |
| Framework web                | Javalin                 | 7.2.0  |
| Banco de dados               | PostgreSQL              | 18     |
| Serviços                     | Docker                  | 4.66.1 |
| Compilação                   | Gradle Wrapper          | 9.4.0  |
| Migrações SQL                | Liquibase               | 5.0.2  |
| Explorador de banco de dados | DBeaver                 | última |
| Testes de API                | Postman                 | última |
| Editor de código             | IntelliJ IDEA Community | última |

---

## Pré-requisitos

Antes de clonar o repositório, certifique-se de ter instalado:

- WSL 2 com Ubuntu (apenas Windows)
- JDK 25
- Git
- Docker Desktop
- IntelliJ IDEA Community Edition
- DBeaver
- Postman

---

## Configuração do ambiente

### Windows — instalação do WSL

O WSL (Windows Subsystem for Linux) permite rodar um terminal Linux dentro do Windows. Todo o desenvolvimento será feito por dentro dele.

Abra o PowerShell como administrador e execute:

```powershell
wsl --install
```

Reinicie o computador quando solicitado. Após reiniciar, o Ubuntu será instalado automaticamente. Configure seu usuário e senha quando pedido.

Para abrir o terminal Ubuntu nas próximas vezes, basta pesquisar "Ubuntu" no menu iniciar.

> Todos os comandos de terminal a partir daqui devem ser executados dentro do Ubuntu, não no PowerShell.

---

### Instalação do JDK 25

Dentro do terminal Ubuntu, execute os comandos abaixo em ordem:

```bash
sudo apt update && sudo apt upgrade -y
```

```bash
sudo apt install -y wget apt-transport-https
```

Adicione o repositório do Adoptium:

```bash
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo apt-key add -
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
```

```bash
sudo apt update
sudo apt install -y temurin-25-jdk
```

Verifique se foi instalado corretamente:

```bash
java -version
```

Deve aparecer algo como:
openjdk version "25" ...

---

### Instalação do Git

```bash
sudo apt install -y git
```

Configure seu nome e e-mail com os mesmos dados do GitHub:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"
```

---

### Instalação do Docker Desktop

O Docker permite rodar o banco de dados PostgreSQL como um contêiner — sem precisar instalar e configurar o PostgreSQL diretamente na sua máquina. Isso garante que o ambiente de todos seja idêntico.

1. Acesse [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
2. Baixe a versão para o seu sistema operacional
3. Execute o instalador e siga os passos
4. No Windows, mantenha a opção **"Use WSL 2 instead of Hyper-V"** marcada
5. Reinicie o computador se pedido
6. Abra o Docker Desktop e aguarde o ícone da baleia aparecer na barra de tarefas

Verifique a instalação:

```bash
docker --version
docker compose version
```

> O Docker Desktop precisa estar aberto e rodando sempre que você for trabalhar no projeto.

---

### Instalação do IntelliJ IDEA Community

1. Acesse [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
2. Baixe a versão **Community Edition** (gratuita)
3. Execute o instalador e siga os passos
4. No Windows, marque **"Add to PATH"** e **"Associate .java files"**

---

### Instalação do DBeaver

O DBeaver é usado para visualizar e consultar o banco de dados PostgreSQL diretamente.

1. Acesse [https://dbeaver.io/download](https://dbeaver.io/download)
2. Baixe a versão **Community** para o seu sistema operacional
3. Execute o instalador e siga os passos

Para conectar ao banco local após subir o Docker, crie uma nova conexão PostgreSQL com as credenciais definidas no `docker-compose.yml`.

---

### Instalação do Postman

O Postman é usado para testar os endpoints da API manualmente.

1. Acesse [https://www.postman.com/downloads](https://www.postman.com/downloads)
2. Baixe e instale para o seu sistema operacional
3. Crie uma conta gratuita quando solicitado

---

### Configurando o terminal WSL no IntelliJ

Por padrão o IntelliJ usa o terminal do Windows. Vamos trocar para o Ubuntu do WSL:

1. Abra o IntelliJ
2. Vá em `File → Settings → Tools → Terminal`
3. No campo **Shell path**, substitua o valor atual por:
   wsl.exe

4. Clique em **OK**
5. Abra o terminal integrado com `Alt + F12`

> A partir daqui, todos os comandos Git e Gradle devem ser executados nesse terminal dentro do IntelliJ.

---

## Clonando o repositório

```bash
git clone https://github.com/eCommerce-UniBH/uni-library.git
cd uni-library
```

Verifique que você está na branch `develop`:

```bash
git branch
```

---

## Executando o projeto

### 1. Suba o banco de dados

```bash
docker compose up -d
```

Para confirmar que está rodando:

```bash
docker compose ps
```

Deve aparecer o contêiner `library-db` com status `running`.

Para derrubar o banco quando terminar:

```bash
docker compose down
```

> Os dados são preservados entre sessões. O `docker compose down` apenas para o contêiner — não apaga nada.

### 2. Compile e execute a aplicação

```bash
./gradlew build
./gradlew run
```

A API estará disponível em `http://localhost:8080`.

---

## Fluxo de trabalho com Git

> **Regra principal: ninguém trabalha diretamente na `develop`. Todo trabalho acontece em uma branch separada.**

**1. Atualize sua `develop` local antes de começar:**

```bash
git checkout develop
git pull origin develop
```

**2. Crie uma branch para sua tarefa:**
[tipo]/LIB-[número da issue]

| Tipo       | Quando usar                       |
|------------|-----------------------------------|
| `feature`  | Nova funcionalidade               |
| `fix`      | Correção de bug                   |
| `refactor` | Refatoração de código             |
| `docs`     | Documentação                      |
| `chore`    | Configuração, build, dependências |

Exemplos:

```bash
git checkout -b feature/LIB-12
git checkout -b fix/LIB-34
```

**3. Faça suas alterações e commit:**

```bash
git add .
git commit -m "feat: adiciona endpoint de cadastro de livro"
```

**4. Envie sua branch:**

```bash
git push origin feature/LIB-12
```

**5. Abra um Pull Request.**

---

## Abrindo um Pull Request

1. Acesse o repositório no GitHub
2. Clique em **"Compare & pull request"**
3. Confirme que o destino é a branch `develop`
4. Preencha o título e a descrição
5. Clique em **"Create pull request"**

> Nunca faça o merge do seu próprio Pull Request.

---

## Checklist antes de abrir um PR

- [ ] O projeto compila sem erros (`./gradlew build`)
- [ ] Estou na branch correta, não na `develop`
- [ ] O destino do PR é a branch `develop`
- [ ] O commit tem prefixo correto (`feat:`, `fix:`, etc.)
- [ ] A descrição do PR explica o que foi feito
- [ ] Não há arquivos de configuração local no commit (`.idea/`, `.env`)

---

## Convenção de commits

| Prefixo     | Quando usar                              |
|-------------|------------------------------------------|
| `feat:`     | Nova funcionalidade                      |
| `fix:`      | Correção de bug                          |
| `refactor:` | Refatoração sem mudança de comportamento |
| `docs:`     | Alteração apenas em documentação         |
| `chore:`    | Configuração, build, dependências        |

Exemplos:
feat: adiciona endpoint de cadastro de livro
fix: corrige cálculo de multa por atraso
docs: atualiza instruções de instalação no README
chore: adiciona dependência do Liquibase no build.gradle

---

## Quadro de tarefas — como usar

Acesse em **Projects → Quadro de tarefas** dentro da organização no GitHub.

| Coluna           | Significado                     |
|------------------|---------------------------------|
| **Backlog**      | Tarefas ainda não iniciadas     |
| **Em andamento** | Você está desenvolvendo         |
| **Em revisão**   | PR aberto, aguardando aprovação |
| **Concluído**    | Merge realizado na `develop`    |

### Fluxo correto

1. Pegue uma issue do **Backlog** e se atribua a ela
2. Mova o card para **Em andamento**
3. Crie sua branch e comece a trabalhar
4. Ao abrir o PR, mova para **Em revisão**
5. Após o merge, o card vai para **Concluído**

### Linkando seu PR à issue
Closes #42

---

## Estrutura do projeto
uni-library/
src/
main/
java/
br/unibh/
book/
api/
service/
repository/
member/
api/
service/
repository/
loan/
api/
service/
repository/
fine/
api/
service/
repository/
resources/
db/changelog/
docs/
diagrams/
use-case.png
class-diagram.png
adr/
001-modular-monolith-arch.md
002-stack.md
.github/
ISSUE_TEMPLATE/
bug.md
feature.md
refactor.md
pull_request_template.md
build.gradle
CODEOWNERS
gradlew
docker-compose.yml
README.md

Cada pacote representa um domínio do sistema. Nenhum código deve ser criado fora dessa estrutura sem alinhamento prévio com o tech lead.

---

## Regras de convivência no repositório

- Nunca trabalhe diretamente na `develop` ou `main`
- Nunca execute `git push --force`
- Nunca aprove o seu próprio Pull Request
- Nunca faça merge sem aprovação do tech lead
- Nunca commite arquivos de configuração local como `.idea/`, `.env` ou `*.iml`
- Sempre atualize sua `develop` local antes de criar uma nova branch

---

## O que fazer quando travar

1. Tente resolver por conta própria por pelo menos 20 minutos
2. Pesquise o erro no Google — copie a mensagem de erro exata
3. Consulte a documentação oficial do que está usando
4. Se ainda não resolveu, leve ao tech lead com: o que você tentou, o erro completo e o que você já pesquisou

---

## Glossário

| Termo                 | Significado                                                        |
|-----------------------|--------------------------------------------------------------------|
| **Branch**            | Uma linha de desenvolvimento isolada dentro do repositório         |
| **Commit**            | Um registro salvo de alterações no código                          |
| **Push**              | Enviar seus commits locais para o GitHub                           |
| **Pull**              | Baixar as atualizações do GitHub para sua máquina                  |
| **Merge**             | Unir o código de uma branch com outra                              |
| **Pull Request (PR)** | Solicitação formal para mesclar seu código na branch principal     |
| **Review**            | Revisão do código feita pelo tech lead antes do merge              |
| **Conflito**          | Quando duas pessoas alteraram o mesmo trecho de código             |
| **Issue**             | Uma tarefa registrada no GitHub                                    |
| **Tech lead**         | Responsável técnico pelo projeto — revisa e aprova todo código     |
| **Docker**            | Ferramenta que empacota e isola serviços em contêineres            |
| **Contêiner**         | Um processo isolado rodando dentro do Docker                       |
| **docker compose**    | Ferramenta para subir múltiplos contêineres com um único comando   |
| **Liquibase**         | Ferramenta que cria e versiona as tabelas do banco automaticamente |
| **DBeaver**           | Ferramenta para visualizar e consultar o banco de dados            |
| **Postman**           | Ferramenta para testar os endpoints da API manualmente             |
| **Migration**         | Script SQL versionado que cria ou altera tabelas no banco          |
| **ADR**               | Documento que registra uma decisão arquitetural e seus motivos     |

---

## Dúvidas Frequentes

**Esqueci de criar uma branch e trabalhei direto na `develop`. O que faço?**

Não commite ainda. Crie a branch agora:

```bash
git checkout -b feature/LIB-12
```

Suas alterações não commitadas vão junto automaticamente.

---

**Meu `git push` foi rejeitado. O que significa?**

```bash
git checkout develop
git pull origin develop
git checkout feature/LIB-12
git merge develop
```

Resolva os conflitos se houver e tente o push novamente.

---

**Como resolvo um conflito de merge?**
<<<<<<< HEAD
seu código
código da develop







develop








Edite o arquivo manualmente e depois:

```bash
git add .
git commit -m "fix: resolve conflito de merge"
```

---

**O tech lead pediu ajustes no meu PR. O que faço?**

```bash
git add .
git commit -m "fix: ajustes solicitados na revisão"
git push origin feature/LIB-12
```

---

**Como sei qual issue pegar?**

Acesse o Quadro de tarefas no GitHub Projects e pegue uma issue do **Backlog** sem ninguém atribuído. Se tiver dúvida, pergunte no Discord.

---

**Posso commitar os arquivos `.idea/` que o IntelliJ criou?**

Não. Eles já estão no `.gitignore` — o Git os ignora automaticamente.

---

**O projeto não conecta no banco de dados. O que faço?**

```bash
docker compose ps
```

Se o contêiner não estiver `running`:

```bash
docker compose up -d
```

---

**Perco os dados do banco quando executo `docker compose down`?**

Não. Os dados ficam num volume do Docker. Para apagar de verdade seria `docker compose down -v` — evite isso.