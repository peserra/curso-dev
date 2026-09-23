# clone-tabnews

## Dependências (npm)

Os pacotes usados pelo projeto são registrados no `package.json`, e o `package-lock.json` guarda as versões instaladas. Para encontrar um pacote, pesquise pelo nome em [npmjs.com](https://www.npmjs.com/).

Para adicionar um pacote usado pela aplicação:

```bash
npm install nome-do-pacote
```

Por exemplo, para adicionar o `axios`:

```bash
npm install axios
```

Este projeto usa `next`, `react` e `react-dom`. Para adicionar esses pacotes juntos:

```bash
npm install next react react-dom
```

Para adicionar uma ferramenta usada apenas durante o desenvolvimento, como formatadores ou ferramentas de teste:

```bash
npm install --save-dev nome-do-pacote
```

`--save-dev` também pode ser escrito como `-D`. O npm registra esses pacotes em `devDependencies` no `package.json`.

Depois de clonar o projeto, instale os pacotes já registrados nele com:

```bash
npm install
```

## Git

- Cada commit gera um novo apontamento para a versao mais atual do arquivo, com um identificador hash SHA1 do conteudo dele
- comando para ver commits:

```bash
    git log
```

- git nao armazena diff, ele calcula sob demanda, joga um blob sobre outro e compara

### Status do controle de versão

```bash
    git status
```

- **Untracked:** Arquivo nunca foi adicionado ao git.
- **Modified:** Arquivo ja estava no repositorio é alterado.

```bash
    git add
```

- **Stageded:** Este arquivo esta pronto para commit.

```bash
    git commit -m "message"
```

- **Commited:** Arquivo sofre o commit.

- caso eu queira alterar um commit:

```bash
    git commit --amend -m "message"
```

ele emenda com o ultimo commit e altera (o commit anterior deixa de existir)

### Git ignore

- Usado para ignorar pastas e arquivos que nao queremos versionamento

## Deploy

### Server e Client

Depois de fazer mudanças locais e jogar no origin remoto, precisamos fazer um deploy,
cada nova mudança é um novo deploy que atualiza o server

arquitetura client/server
client: pede
servidor : entrega

um server pode ter multiplos clients e conversar com multiplos servicos, basta que se respeitem
os protocolos

um server intermediario é chamado de proxy

### Hospedagem e deploy

Ao inves de mandar do pc pessoal para o servidor, por que não editar no ambiente de produção?

Windows server no windows ou SSH no linux, para conectar direto com o server

ambiente local e remoto podiam ser diferentes, e muitas vezes tinham problemas

hoje em dia, desenvolve se localmente, manda para um C.I (continuous integrator) que testa
as mudanças. Se nada quebrou, envia para outra maquina que vai fazer o Build, enviando
para os servidores na internet.

#### Como dar um deploy

###### Vercel
