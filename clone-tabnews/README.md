# clone-tabnews

Projeto de estudo para reproduzir o TabNews e praticar desenvolvimento web. Este README é um guia para lembrar o que foi configurado, como trabalhar no projeto e por que cada escolha foi feita.

## Começar a desenvolver

O projeto usa Node.js, npm, Next.js e React. Para instalar as dependências e iniciar o servidor local:

```bash
npm install
npm run dev
```

O Next.js inicia o site localmente e atualiza a página durante o desenvolvimento. A rota inicial está em `pages/index.js`.

### Node.js

O arquivo `.nvmrc` registra `lts/krypton`, para que o [nvm](https://github.com/nvm-sh/nvm) possa selecionar a versão LTS de Node.js do projeto:

```bash
nvm install
nvm use
```

Isso ajuda a manter a mesma versão do Node entre máquinas. Se não uso nvm, preciso instalar uma versão compatível manualmente.

## Dependências npm

As dependências usadas pela aplicação ficam em `dependencies` no `package.json`. As ferramentas usadas só durante o desenvolvimento ficam em `devDependencies`. O `package-lock.json` registra a árvore e as versões resolvidas para que as instalações sejam reproduzíveis. Os dois arquivos devem ser mantidos no Git.

Dependências atuais da aplicação:

- `next`: framework da aplicação web, incluindo o servidor de desenvolvimento e o sistema de rotas baseado na pasta `pages`.
- `react` e `react-dom`: criação e renderização da interface React.
- `prettier` (desenvolvimento): formatação consistente do código e dos arquivos do projeto.

Para adicionar uma dependência usada pela aplicação:

```bash
npm install nome-do-pacote
```

Para adicionar uma ferramenta de desenvolvimento, como um formatador ou ferramenta de teste:

```bash
npm install --save-dev nome-do-pacote
```

`--save-dev` também pode ser escrito como `-D`. O npm atualiza `package.json` e `package-lock.json`; não é necessário editar o lockfile manualmente. Para instalar o que já está registrado depois de clonar:

```bash
npm install
```

## Scripts do projeto

Os comandos abaixo estão definidos em `scripts` no `package.json`:

| Comando              | O que faz                           | Por que usar                                                           |
| -------------------- | ----------------------------------- | ---------------------------------------------------------------------- |
| `npm run dev`        | Inicia o servidor local do Next.js. | Desenvolver e conferir o site no navegador.                            |
| `npm run lint:check` | Executa `prettier --check .`.       | Verifica se os arquivos estão formatados sem alterá-los.               |
| `npm run lint:fix`   | Executa `prettier --write .`.       | Formata os arquivos compatíveis antes de revisar ou commitar mudanças. |

O Prettier foi instalado como dependência de desenvolvimento porque só é necessário para trabalhar no código, não para executar a aplicação em produção. Os scripts deixam a verificação e a formatação fáceis de repetir. Depois de editar, posso executar:

```bash
npm run lint:check
```

Se houver arquivos para formatar:

```bash
npm run lint:fix
```

## Organização das páginas

O Next.js usa a pasta `pages` para criar rotas. A convenção está anotada em `pages/README.md`:

| Arquivo                    | Rota               |
| -------------------------- | ------------------ |
| `pages/index.js`           | `/`                |
| `pages/produtos/index.js`  | `/produtos`        |
| `pages/recuperar-senha.js` | `/recuperar-senha` |

`pages/index.js` exporta o componente React `Home`, que por enquanto mostra uma mensagem de exemplo. Novas páginas devem seguir a convenção de nomes e exportar o componente correspondente.

## Consistência do editor

`.editorconfig` é a configuração compartilhada pelos editores: usa espaços e indentação de dois caracteres. Isso reduz diferenças de formatação quando o projeto é aberto em editores diferentes. O Prettier complementa essa configuração ao formatar os arquivos.

## Arquivos ignorados pelo Git

O `.gitignore` lista o que pode ser recriado ou não deve ser compartilhado:

- `node_modules`: pacotes instalados localmente (`npm install` recria).
- `.next`, `out`: saída gerada pelo Next.js.
- `next-env.d.ts`: tipos gerados pelo Next.js; ele recria o arquivo ao rodar `npm run dev` ou `next build`.
- `*.tsbuildinfo`: cache incremental do TypeScript.
- `.vercel`: vínculo local com o projeto na Vercel.
- `.env`, `.env.*` (exceto `.env.example`): variáveis e segredos locais, que nunca devem ir para o Git.
- `*.log`, `npm-debug.log*`, `coverage`: logs e relatórios de teste.
- `.DS_Store`, `.idea`: arquivos do macOS e do editor.

Uso `!.env.example` para poder versionar um exemplo sem valores reais.

**Atenção para este repositório:** `node_modules` foi adicionado ao Git antes da criação do `.gitignore`. O Git continua rastreando arquivos que já estavam versionados; adicioná-los ao `.gitignore` não os remove do histórico nem do índice. Para parar de rastrear a pasta sem apagar a cópia local, executar a partir de `clone-tabnews`:

```bash
git rm -r --cached node_modules
```

Depois, revisar e commitar essa remoção. A regra existente no `.gitignore` evita que os arquivos voltem a ser adicionados.

## Git: fluxo de trabalho

O repositório Git está na pasta que contém `clone-tabnews`. Posso executar os comandos Git dentro da pasta do projeto; para ver o estado de tudo que está no repositório, também posso usar `git status`.

1. Conferir os arquivos alterados antes de preparar o commit:

   ```bash
   git status
   git diff
   ```

2. Adicionar as mudanças que quero incluir e conferir o que ficou preparado:

   ```bash
   git add README.md package.json package-lock.json pages
   git diff --staged
   ```

   Posso acrescentar outros caminhos ao `git add` ou usar `git add -A` para preparar todas as mudanças do repositório. Conferir o status evita incluir alterações por engano.

3. Criar um commit com uma mensagem que resuma a mudança:

   ```bash
   git commit -m "documenta configuracao do projeto"
   ```

4. Enviar o commit para a branch principal no remoto `origin`:

   ```bash
   git push origin main
   ```

`git status` mostra arquivos _untracked_ (ainda não rastreados), _modified_ (alterados) e _staged_ (preparados para o próximo commit). `git log --oneline` lista o histórico de commits. Um commit identifica uma versão do projeto; o Git compara versões a partir de seus objetos, em vez de guardar um diff simples por arquivo como fonte primária.

`git commit --amend -m "nova mensagem"` substitui o commit mais recente, incluindo alterações preparadas. Como isso reescreve o histórico, usar apenas antes de compartilhar o commit; um commit já enviado exige cuidado extra para sincronizar o remoto.

## Deploy: anotações e próximos passos

O navegador atua como _client_: faz solicitações. O servidor recebe essas solicitações e devolve respostas; ele também pode conversar com outros serviços seguindo protocolos. Um servidor que encaminha solicitações entre cliente e outros servidores pode atuar como _proxy_.

Desenvolver localmente ajuda a manter o ambiente de desenvolvimento separado do de produção. Em um fluxo de CI/CD, as mudanças podem ser testadas e compiladas antes de serem publicadas. CI significa _continuous integration_; CD significa _continuous delivery_ ou _continuous deployment_.

A ideia anotada para hospedar este projeto é usar a Vercel: criar uma conta, conectar o repositório GitHub e configurar o projeto Next.js. Com essa integração configurada, um `git push` pode iniciar um novo deploy automaticamente. As notas neste README registram o plano; a integração em si depende de configurar o projeto na Vercel.

## Histórico do que foi montado

- Foi iniciado o repositório e criado este guia para guardar notas de desenvolvimento, Git e deploy.
- O projeto foi colocado em `clone-tabnews` e `.nvmrc` foi adicionado para registrar a versão LTS de Node.js usada durante o curso.
- Foram adicionados Next.js, React e React DOM, junto com o `package-lock.json`, para criar e executar a aplicação.
- Foi criada a pasta `pages` e sua documentação de rotas. `pages/index.js` passou a exportar a página inicial de exemplo.
- Foi adicionado `.gitignore` para ignorar a saída do Next.js e as dependências locais; ver a observação acima sobre `node_modules` que já era rastreado.
- Foi adicionado `.editorconfig` para padronizar indentação com espaços e largura de dois caracteres.
- Foi instalado Prettier em `devDependencies` e adicionados `lint:check` e `lint:fix` ao `package.json` para verificar e corrigir a formatação.

- O `.gitignore` foi ampliado com `next-env.d.ts`, `*.tsbuildinfo`, `.env*`, `.vercel`, logs, `coverage`, `.DS_Store` e `.idea`.

Ao mudar uma dessas configurações no futuro, atualizar este guia com o que mudou, o motivo e o comando necessário para reproduzir a configuração.
