# Guia de Sincronização de Projetos — GitHub

Este documento explica, **passo a passo**, como manter todos os seus projetos sincronizados entre o **PC de desenvolvimento** e o **notebook**, usando o GitHub como ponte.

---

## 1. Visão geral

Todos os seus projetos estão em **7 repositórios privados** na conta:

```
https://github.com/efraimrodrigo-alltech
```

| Pasta local (`C:\Desenvolvimento\...`) | Repositório no GitHub |
|---|---|
| `Projeto SaaS` | `https://github.com/efraimrodrigo-alltech/projeto-saas` |
| `Projeto_Allegretto` | `https://github.com/efraimrodrigo-alltech/projeto-allegretto` |
| `Projeto_Alltech` | `https://github.com/efraimrodrigo-alltech/projeto-alltech` |
| `Projeto_Bar e Restaurantes` | `https://github.com/efraimrodrigo-alltech/projeto-bar-e-restaurantes` |
| `Projeto_Barbearia` | `https://github.com/efraimrodrigo-alltech/projeto-barbearia` |
| `Projeto_Lavanderia` | `https://github.com/efraimrodrigo-alltech/projeto-lavanderia` |
| `Projeto_Tatu` | `https://github.com/efraimrodrigo-alltech/projeto-tatu` |

> Todos são **PRIVADOS** — só a conta `efraimrodrigo-alltech` tem acesso.

**Como funciona:** você edita os arquivos localmente, "empacota" as mudanças (`commit`), manda para o GitHub (`push`) e, na outra máquina, baixa as mudanças (`pull`). O GitHub guarda o histórico completo de tudo.

---

## 2. Pré-requisitos (uma vez por máquina)

Você precisa destes dois programas instalados na máquina que for usar:

- **Git** — já instalado em: `C:\Program Files\Git\cmd\git.exe`
- **GitHub CLI (`gh`)** — já instalado em: `C:\Program Files\GitHub CLI\gh.exe`

### 2.1 Verificar se o Git funciona

Abra o **PowerShell** e digite:

```powershell
git --version
```

Se aparecer algo como `git version 2.x.x`, está OK.

> Se der erro "não é reconhecido", o Git não está no caminho (PATH). Nesse caso, use sempre o caminho completo:
> ```powershell
> & "C:\Program Files\Git\cmd\git.exe" --version
> ```

### 2.2 Login no GitHub (uma vez por máquina)

```powershell
gh auth login
```

Responda assim na tela interativa:

1. **Where do you use GitHub?** → `GitHub.com`
2. **Preferred protocol?** → `HTTPS`
3. **Authenticate Git with your GitHub credentials?** → `Yes`
4. **How would you like to authenticate?** → `Login with a web browser`
5. Pressione **Enter** → abrirá o navegador
6. No navegador: cole o código que aparece no terminal → **Continue**
7. **Faça login com a conta `efraimrodrigo-alltech`** (e-mail + senha)
8. Clique em **Authorize** quando pedir acesso para o "GitHub CLI"

**Para confirmar que funcionou:**

```powershell
gh auth status
```

Deve mostrar: `✓ Logged in to github.com account efraimrodrigo-alltech`.

> **IMPORTANTE:** o login só precisa ser feito **uma vez por máquina**. Depois disso, o GitHub CLI lembra. Isso faz o `git push` e `git pull` funcionarem sem pedir senha toda vez.

---

## 3. Configuração na máquina nova (notebook) — primeira vez

No notebook, crie a mesma estrutura de pastas e baixe os projetos:

### 3.1 Criar a pasta de trabalho

```powershell
New-Item -ItemType Directory -Path "C:\Desenvolvimento"
```

### 3.2 Clonar cada projeto

Para **cada** projeto, rode (troque o nome ao final de cada linha):

```powershell
cd C:\Desenvolvimento

git clone https://github.com/efraimrodrigo-alltech/projeto-saas.git "C:\Desenvolvimento\Projeto SaaS"
git clone https://github.com/efraimrodrigo-alltech/projeto-allegretto.git "C:\Desenvolvimento\Projeto_Allegretto"
git clone https://github.com/efraimrodrigo-alltech/projeto-alltech.git "C:\Desenvolvimento\Projeto_Alltech"
git clone "https://github.com/efraimrodrigo-alltech/projeto-bar-e-restaurantes.git" "C:\Desenvolvimento\Projeto_Bar e Restaurantes"
git clone https://github.com/efraimrodrigo-alltech/projeto-barbearia.git "C:\Desenvolvimento\Projeto_Barbearia"
git clone https://github.com/efraimrodrigo-alltech/projeto-lavanderia.git "C:\Desenvolvimento\Projeto_Lavanderia"
git clone https://github.com/efraimrodrigo-alltech/projeto-tatu.git "C:\Desenvolvimento\Projeto_Tatu"
```

> O nome entre aspas no final é **importante** para projetos com espaço no nome (`Projeto SaaS` e `Projeto Bar e Restaurantes`).

### 3.3 Criar os arquivos `.env` (segredos)

Os arquivos `.env` **não** foram enviados ao GitHub por segurança. Eles contêm senhas, tokens do Mercado Pago, chaves do Supabase, etc.

**No notebook, você precisa recriá-los.** Copie de cada pasta um modelo:

- `Projeto SaaS\SaaS\.env.example` → renomeie/copie para `Projeto SaaS\SaaS\.env`
- `Projeto SaaS\SaaS\php-project\.env.example` → `Projeto SaaS\SaaS\php-project\.env`
- `Projeto_Alltech\alltech\Site_AllTech\.env.example` → `...\Site_AllTech\.env`
- `Projeto_Bar e Restaurantes\Site\php-app\.env.example` → `...\php-app\.env`
- `Projeto_Barbearia\Barbearia_APP\.env.example` → `...\Barbearia_APP\.env`
- `Projeto_Barbearia\Barbearia_Web\.env.example` → `...\Barbearia_Web\.env`
- `Projeto_Lavanderia\App_Lavanderia_V1\.env.example` → `...\App_Lavanderia_V1\.env`
- `Projeto_Lavanderia\App_Lavanderia_V2\.env.example` → `...\App_Lavanderia_V2\.env`

Depois **preencha com os valores reais** (mesmos valores que estão no `.env` do PC). Assim os projetos rodam no notebook igual ao PC.

> O GitHub nunca recebe o `.env` real. Você preenche manualmente em cada máquina.

---

## 4. Rotina diária de sincronização

### 4.1 No notebook: enviar suas mudanças

No PowerShell, **dentro da pasta do projeto** (ex: `cd C:\Desenvolvimento\Projeto SaaS`):

```powershell
git add -A
git commit -m "descrição das mudanças feitas"
git push
```

- `git add -A` → prepara todos os arquivos alterados/criados
- `git commit -m "..."` → salva o "pacote" de mudanças com uma descrição
- `git push` → envia para o GitHub

**Dica:** substitua a mensagem entre aspas por algo que descreva o que mudou, ex: `git commit -m "corrigi o layout da tela de login"`.

### 4.2 No PC: baixar as mudanças

**Na primeira vez, abra o terminal e sincronize:**

```powershell
cd "C:\Desenvolvimento\Projeto SaaS"
git pull
```

> O PC é a "cópia principal" — o comando `git pull` baixa as mudanças que você fez no notebook.

---

## 5. Fluxo completo (cenário real do dia a dia)

**Cenário: você trabalha no notebook, depois continua no PC.**

1. **No notebook**, quando terminar:
   ```powershell
   cd C:\Desenvolvimento\Projeto SaaS
   git add -A
   git commit -m "trabalho do dia"
   git push
   ```
   > Se aparecer um erro de que há mudanças remotas, veja a seção 7.2.

2. **No PC**, ao abrir o workspace, para cada projeto que mudou:
   ```powershell
   cd "C:\Desenvolvimento\Projeto SaaS"
   git pull
   ```

3. **No PC**, quando terminar de editar:
   ```powershell
   git add -A
   git commit -m "ajustes feitos no PC"
   git push
   ```

4. **No notebook**, antes de continuar:
   ```powershell
   cd C:\Desenvolvimento\Projeto SaaS
   git pull
   ```

> **Regra de ouro:** **SEMPRE faça `git pull` ANTES de editar** na máquina onde você vai trabalhar, para não trabalhar em cima de arquivos antigos.

---

## 6. Fluxo para máquina nova (caso queira baixar um projeto do zero)

```powershell
cd C:\Desenvolvimento
git clone https://github.com/efraimrodrigo-alltech/projeto-alltech.git "C:\Desenvolvimento\Projeto_Alltech"
cd "C:\Desenvolvimento\Projeto_Alltech"
git pull
```

Pronto — a pasta estará com tudo que existe no GitHub.

---

## 7. Problemas comuns e soluções

### 7.1 `git push` pede usuário/senha ou falha

- Garanta que o login foi feito: `gh auth status`
- Se não estiver logado: `gh auth login` (seção 2.2)

### 7.2 Erro: `git push rejected` / `non-fast-forward`

Significa que o GitHub tem mudanças que a sua máquina não tem (ex: você esqueceu de dar pull).

**Solução:** dentro da pasta do projeto:

```powershell
git pull --no-rebase
```

O Git vai tentar juntar tudo. Se ele reclamar de conflito (arquivos editados nos dois lugares), abra os arquivos indicados, resolva (mantenha o que faz sentido, apague o duplicado), e então:

```powershell
git add -A
git commit -m "resolvi conflito"
git push
```

> Se quiser **manter somente as suas mudanças locais** (jogar fora as do GitHub), use: `git pull --rebase` e depois `git push`. Use com cuidado.

### 7.3 Erro: `git pull` falha com "local changes would be overwritten"

Você editou um arquivo e o GitHub também mudou ele. **Solução:** primeiro guarde suas mudanças, depois puxe, depois devolva:

```powershell
git stash
git pull
git stash pop
```

### 7.4 Quero desfazer o último commit (sem apagar os arquivos)

```powershell
git reset --soft HEAD~1
```

Isso "desfaz" o último commit, mantendo os arquivos como estão. O commit no GitHub continua existindo até você mandar outro push.

### 7.5 Como ver o histórico

```powershell
git log --oneline
```

### 7.6 Como ver o que mudou desde a última sincronização

```powershell
git status
```

### 7.7 O notebook perdeu dados / quero recuperar tudo do GitHub

```powershell
git clone https://github.com/efraimrodrigo-alltech/projeto-saas.git "C:\Desenvolvimento\Projeto SaaS"
```

Isso baixa a última versão completa de tudo que foi enviado.

---

## 8. Segurança — o que NUNCA vai para o GitHub

Estes itens são **automaticamente ignorados** pelos arquivos `.gitignore` e **não** podem ser enviados:

- Arquivos `.env` (senhas, tokens de API, chaves de banco)
- Pastas `node_modules` (dependências do Node — são baixadas de novo com `npm install`)
- Pastas `vendor` (dependências do PHP — recriadas com `composer install`)
- Arquivos grandes `.zip` e `.apk`
- Pastas de build como `dist`, `build`, `__pycache__`

**Por isso:**
- No notebook, após clonar, você precisa rodar os instaladores das dependências:
  - Projetos Node: `npm install` (dentro da pasta do site)
  - Projetos PHP: `composer install` (dentro da pasta com `composer.json`)
  - Depois recriar os `.env` (seção 3.3)

**Nunca** tente forçar o envio de arquivos ignorados com `git add -f`. Isso pode vazar senhas para o GitHub.

---

## 9. Backup duplo (recomendado)

O GitHub é um ótimo backup remoto, mas não é a única cópia que você deve ter:

- Mantenha o PC como **cópia principal**
- Faça **cópia manual em HD externo** dos projetos importantes de vez em quando (principalmente os que têm `.env` e dados de clientes)
- O GitHub só guarda o que foi enviado via `push`

---

## 10. Comandos rápidos (colinha)

| Comando | O que faz |
|---|---|
| `git status` | mostra o que mudou |
| `git add -A` | prepara todas as mudanças |
| `git commit -m "msg"` | salva as mudanças com descrição |
| `git push` | envia para o GitHub |
| `git pull` | baixa mudanças do GitHub |
| `git log --oneline` | histórico |
| `git stash` / `git stash pop` | guarda / devolve mudanças temporariamente |
| `gh auth status` | confirma se está logado |
| `gh auth login` | faz login (uma vez por máquina) |

---

## Resumo do passo a passo

1. **Cada máquina:** instale Git + GitHub CLI, rode `gh auth login` (uma vez)
2. **Notebook (primeira vez):** clone os 7 projetos + recrie os `.env` + rode `npm install`/`composer install`
3. **Rotina:** `git pull` antes de editar → editar → `git add -A` → `git commit -m "..."` → `git push`
4. **Na outra máquina:** `git pull` para receber tudo

Dúvidas sobre qualquer passo, basta perguntar.
