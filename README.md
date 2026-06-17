# Construtor de Genealogia

Aplicação estática em HTML/JavaScript para abrir, visualizar, editar e guardar árvores GEDCOM. A aplicação funciona no navegador, sem servidor próprio.

## O problema das fotografias

Um ficheiro GEDCOM normalmente **não contém os bytes da fotografia**. Ele guarda apenas uma referência, por exemplo:

```text
1 OBJE
2 FILE ./media/retrato-joao.jpg
3 FORM jpg
3 TITL Retrato de João
2 _PRIM Y
```

Por isso, descarregar apenas o novo `.ged` não basta para manter as imagens. É necessário guardar também os ficheiros da pasta `media/`.

## Estrutura recomendada

```text
tree/
├── index.html
├── project.json              # criado automaticamente ao salvar o projeto
├── README.md
├── .nojekyll
├── .gitignore
├── data/
│   └── familia.ged           # criado/atualizado pela aplicação
└── media/
    ├── retrato-joao.jpg
    └── certidao-casamento.pdf
```

## Fluxo correto com um repositório Git local

1. Clone o repositório `tree` para o computador usando GitHub Desktop.
2. Abra o site publicado ou o `index.html` por um servidor local.
3. Clique em **Projeto local**.
4. Selecione a pasta local clonada do repositório `tree`.
5. Abra ou carregue o GEDCOM.
6. Edite pessoas, relações e mídias.
7. Ao adicionar uma fotografia, marque **Usar como foto principal** quando necessário.
8. Clique em **Salvar projeto**.
9. A aplicação grava:
   - `data/familia.ged`;
   - os ficheiros associados em `media/`;
   - `project.json`, com os caminhos do projeto.
10. Abra o GitHub Desktop, faça **Commit** e depois **Push origin**.

O navegador não faz o `push` diretamente para o GitHub. Ele grava na pasta local selecionada. O GitHub Desktop envia essas alterações ao repositório remoto.

## Botões principais

- **Abrir GEDCOM**: abre um `.ged` isolado.
- **Projeto local**: liga ou abre uma pasta local de projeto Git.
- **Salvar projeto**: grava a árvore completa em `data/familia.ged` e copia as mídias para `media/`.
- **Pasta de mídia**: associa temporariamente uma pasta de imagens/documentos já existente.
- **Projeto web**: carrega `project.json` e `data/familia.ged` publicados no mesmo repositório.
- **GEDCOM**: gera um download completo ou filtrado por ramo/família.

## Foto principal

Ao marcar uma imagem como principal, a aplicação grava as tags:

```text
2 _PRIM Y
2 _PERSONALPHOTO Y
```

Na reabertura, a aplicação reconhece essas tags e usa a imagem como retrato principal. O ficheiro também precisa existir em `media/`.

## Um HTML para cada GEDCOM?

Não é necessário criar código diferente para cada árvore. O mesmo `index.html` pode ser reutilizado.

A organização mais simples é:

- um repositório por árvore genealógica;
- o mesmo `index.html` em todos;
- um `project.json` próprio;
- um `data/familia.ged` próprio;
- uma pasta `media/` própria.

Assim, cada repositório pode ter uma URL distinta, mas o programa permanece igual.

## Carregamento automático

Quando `project.json` existe e aponta para `./data/familia.ged`, a página tenta carregar automaticamente a árvore publicada.

Exemplo de `project.json`:

```json
{
  "schema": "genealogia-web-project/1.0",
  "title": "Construção de Genealogia",
  "gedcom": "./data/familia.ged",
  "media": "./media/",
  "autoload": true
}
```

## Privacidade

Se o GitHub Pages estiver público, estes ficheiros também ficam acessíveis publicamente:

```text
data/familia.ged
media/*
```

Isso pode expor nomes, datas, relações familiares, fotografias e documentos de pessoas vivas. Antes do `push`, confirme se o repositório e o site têm o nível de privacidade adequado.

## Compatibilidade do projeto local

A gravação direta numa pasta usa a API de acesso ao sistema de ficheiros do navegador. Para este fluxo, use Chrome ou Edge e abra o site por HTTPS ou por `localhost`.

## Teste local

Na pasta do projeto:

```bash
python -m http.server 8080
```

Depois abra:

```text
http://localhost:8080
```
