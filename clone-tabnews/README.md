# clone-tabnews

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

