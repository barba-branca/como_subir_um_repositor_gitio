
# Guia de Comandos Git: Do Básico ao Avançado

Este guia é um manual de referência para o dia a dia com Git. Ele cobre os comandos mais importantes, o fluxo de trabalho padrão e como solucionar problemas comuns.

## Índice

1.  [Configuração Inicial](https://www.google.com/search?q=%231-configura%C3%A7%C3%A3o-inicial)
2.  [O Fluxo de Trabalho Básico](https://www.google.com/search?q=%232-o-fluxo-de-trabalho-b%C3%A1sico)
3.  [Trabalhando com Branches (Ramificações)](https://www.google.com/search?q=%233-trabalhando-com-branches-ramifica%C3%A7%C3%B5es)
4.  [Trabalhando com Repositórios Remotos](https://www.google.com/search?q=%234-trabalhando-com-reposit%C3%B3rios-remotos)
5.  [Desfazendo Alterações](https://www.google.com/search?q=%235-desfazendo-altera%C3%A7%C3%B5es)
6.  [Solucionando Erros Comuns](https://www.google.com/search?q=%236-solucionando-erros-comuns)

-----

## 1\. Configuração Inicial

  * **Finalidade:** Configurar suas informações de usuário. Isso é feito apenas uma vez na sua máquina.

  * **Comandos:**

    ```bash
    # Configura seu nome de usuário (será exibido nos commits)
    git config --global user.name "Seu Nome"

    # Configura seu e-mail (essencial para o GitHub/GitLab)
    git config --global user.email "seu.email@exemplo.com"
    ```

  * **Se der erro:**

      * **Problema:** O comando não é reconhecido.
      * **Solução:** Verifique se o Git está instalado corretamente e se o `PATH` do sistema inclui o diretório de instalação do Git.

-----

## 2\. O Fluxo de Trabalho Básico

Este é o ciclo que você usará 90% do tempo.

### Passo 1: Iniciar um Repositório

  * **Finalidade:** Criar um novo repositório ou clonar um existente.

  * **Comandos:**

    ```bash
    # Inicia um novo repositório na pasta atual
    git init

    # Ou clona um repositório existente da internet
    git clone https://github.com/usuario/repositorio.git
    ```

### Passo 2: Verificar o Status

  * **Finalidade:** Ver o estado dos seus arquivos (quais estão modificados, quais estão prontos para commit, etc.).

  * **Comando:**

    ```bash
    git status
    ```

### Passo 3: Adicionar Arquivos (Stage)

  * **Finalidade:** Preparar os arquivos que você deseja salvar na "foto" (commit) do projeto.

  * **Comandos:**

    ```bash
    # Adiciona um arquivo específico
    git add nome_do_arquivo.txt

    # Adiciona todos os arquivos modificados e novos no diretório atual
    git add .
    ```

### Passo 4: Fazer o Commit

  * **Finalidade:** Salvar permanentemente as alterações preparadas (staged) no histórico do projeto.

  * **Comando:**

    ```bash
    # Faz o commit com uma mensagem descritiva
    git commit -m "Adiciona funcionalidade de login"
    ```

  * **Se der erro:**

      * **Problema:** "nothing to commit, working tree clean".
      * **Solução:** Você esqueceu de usar `git add` antes de `git commit`. Adicione os arquivos que deseja salvar e tente o commit novamente.

-----

## 3\. Trabalhando com Branches (Ramificações)

  * **Finalidade:** Branches permitem que você trabalhe em novas funcionalidades ou correções de bugs de forma isolada, sem afetar a linha de produção principal (`main`).

  * **Comandos:**

    ```bash
    # Lista todas as branches locais. A branch atual é marcada com um *
    git branch

    # Cria uma nova branch
    git branch nome-da-nova-branch

    # Muda para a branch especificada
    git checkout nome-da-nova-branch

    # Atalho: Cria e já muda para a nova branch
    git checkout -b nome-da-nova-branch

    # Volta para a branch principal
    git checkout main

    # Une as alterações da outra branch na sua branch atual
    git merge nome-da-outra-branch

    # Deleta uma branch local (use -D para forçar se tiver alterações não mescladas)
    git branch -d nome-da-branch-para-deletar
    ```

  * **Se der erro:**

      * **Problema:** Conflito de merge (`MERGE_CONFLICT`).
      * **Solução:** O Git não conseguiu unir as alterações automaticamente.
        1.  Abra os arquivos indicados no `git status` que estão com conflito.
        2.  Procure pelas marcações `<<<<<<<`, `=======`, `>>>>>>>`.
        3.  Edite o arquivo para deixar a versão final que você deseja.
        4.  Remova as marcações do Git.
        5.  Use `git add nome_do_arquivo_corrigido.txt` para marcar o conflito como resolvido.
        6.  Continue o merge com `git commit`.

-----

## 4\. Trabalhando com Repositórios Remotos

  * **Finalidade:** Sincronizar seu trabalho com um repositório remoto (como no GitHub).

  * **Comandos:**

    ```bash
    # Conecta seu repositório local a um repositório remoto
    git remote add origin https://github.com/usuario/repositorio.git

    # Envia seus commits para o repositório remoto
    # O -u cria a ligação entre a branch local e a remota na primeira vez
    git push -u origin main

    # Envia commits de outras branches
    git push origin nome-da-branch

    # Baixa as alterações do repositório remoto para o seu repositório local
    git pull

    # Lista os repositórios remotos configurados
    git remote -v
    ```

  * **Se der erro:**

      * **Problema:** `failed to push some refs` ou `Updates were rejected`.
      * **Solução:** Isso acontece porque existem alterações no repositório remoto que você não tem localmente. Você precisa primeiro baixar essas alterações.
        1.  Use `git pull` para baixar e tentar mesclar as alterações remotas.
        2.  Se houver conflitos, resolva-os (veja a seção de merge).
        3.  Após resolver e fazer o commit, tente o `git push` novamente.

-----

## 5\. Desfazendo Alterações

  * **Finalidade:** Corrigir erros, reverter commits ou descartar alterações.

  * **Comandos:**

    ```bash
    # Descarta alterações em um arquivo que ainda NÃO foi para o stage
    git checkout -- nome_do_arquivo.txt

    # Tira um arquivo do stage, mas mantém as modificações no seu diretório
    git reset HEAD nome_do_arquivo.txt

    # Cria um novo commit que desfaz as alterações de um commit específico
    # É a forma mais segura de reverter algo que já foi para o repositório remoto
    git revert ID_DO_COMMIT

    # Reseta o histórico para um commit anterior. CUIDADO: reescreve o histórico!
    # --soft: Mantém as alterações em stage
    # --hard: Descarta TODAS as alterações. Use com extrema cautela!
    git reset --hard ID_DO_COMMIT

    # Visualiza o histórico de commits de forma simplificada
    git log --oneline --graph --decorate
    ```

  * **Se der erro:**

      * **Problema:** Você usou `git reset --hard` e perdeu trabalho.
      * **Solução:** O Git guarda um log de tudo. O comando `git reflog` mostra o histórico de movimentações do `HEAD`. Você pode encontrar o hash do commit antes do reset e usar `git reset --hard HASH_RECUPERADO` para voltar a esse estado.

-----

## 6\. Solucionando Erros Comuns

| Erro | O que significa? | Como resolver? |
| :--- | :--- | :--- |
| **`fatal: not a git repository...`** | Você tentou rodar um comando Git fora de uma pasta que é um repositório. | Navegue até a pasta correta do seu projeto ou inicie um novo repositório com `git init`. |
| **`Your branch is ahead of 'origin/main'...`** | Você tem commits locais que ainda não foram enviados para o repositório remoto. | Isso é apenas um aviso. Use `git push` para enviar suas alterações. |
| **`Updates were rejected because the remote contains work that you do not have locally.`** | Alguém (ou você, em outra máquina) enviou alterações para o repositório antes de você. | 1. Execute `git pull` para baixar as alterações remotas. \<br\> 2. Resolva eventuais conflitos. \<br\> 3. Execute `git push` novamente. |
| **`fatal: remote origin already exists.`** | Você tentou adicionar um repositório remoto (`origin`) que já está configurado. | Você pode verificar os remotos com `git remote -v`. Se precisar mudar a URL, use `git remote set-url origin NOVA_URL`. |
| **`error: pathspec '...' did not match any file(s) known to git.`** | Você digitou o nome de um arquivo ou branch incorretamente. | Verifique se o nome do arquivo ou branch está correto. Lembre-se que o Git diferencia maiúsculas de minúsculas na maioria dos sistemas. |
| **`detached HEAD state`** | Você fez checkout de um commit específico em vez de uma branch. Você está "flutuando" no histórico. | Se você quer apenas olhar o código, não há problema. Se você quer trabalhar a partir daqui, crie uma nova branch: `git checkout -b nome-da-nova-branch`. |
