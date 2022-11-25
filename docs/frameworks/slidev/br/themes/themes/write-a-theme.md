# Escrever um Tema

Pra começar, nós te recomendamos usar nosso gerador como base para o seu primeiro tema:

```bash
$ npm init slidev-theme
```

Depois você pode modificar e brincar com o tema. Você também pode consultar os [temas oficiais](themes/gallery) como exemplos.

## Capacidade

Um tema pode contribuir nos seguintes pontos:

- Estilos globais
- Prover configurações padrão (fontes, esquema de cor, destacadores, etc.)
- Prover layouts customizados ou sobrepor um existentes
- Prover componentes customizados ou sobrepor existentes
- Estender as configurações do Windi CSS
- Configurar ferramentas como Monaco e Prism

## Convenções

Temas são publicados no registro do npm, e eles devem seguir as seguintes convenções:

- Nome do pacote deve começar com `slidev-theme-`, por exemplo: `slidev-theme-incrivel`
- Adicione `slidev-theme` e `slidev` no campo `keywords` do seu `package.json`

## Configuração

Para configurar o ambiente de teste para o seu tema, você pode criar um arquivo `example.md` com o seguinte frontmatter, para dizer ao Slidev que você está usando o diretório atual como um tema.

```md
---
theme: ./
---
```

Opcionalmente, você também pode adicionar alguns scripts ao seu `packages.json`

```json
// package.json
{
  "scripts": {
    "dev": "slidev example.md",
    "build": "slidev build example.md",
    "export": "slidev export example.md",
    "screenshot": "slidev export example.md --format png"
  }
}
```

Para publicar seu tema, simplesmente execute `npm publish` e pronto. Não há nenhum processo de compilação necessário (o que significa que você pode publicar arquivos `.vue` e `.ts` diretamente, o Slidev é esperto o bastante para entendê-los).

Os pontos de contribuição do tema seguem as mesmas convenções das customizações locais, consulte [a documentação das convenções de nome](custom/).

## Configurações Padrão

> Disponível desde a v0.19

Um tema pode prover [configurações](custom/.html) padrão por meio do `package.json`.

```json
// package.json
{
  "slidev": {
    "default": {
      "aspectRatio": "16/9",
      "canvasWidth": 980,
      "fonts": {
        "sans": "Robot",
        "mono": "Fira Code"
      }
    }
  }
}
```

Fontes serão automaticamente importadas do [Google Fonts](https://fonts.google.com/).

Aprenda mais sobre [fontes](custom/fonts) e [configurações do frontmatter](/custom/.html)

## Metadados do Tema

### Esquema de Cores

Por padrão, o Slidev assume que os temas suportam tanto o modo claro quanto o escuro. Se você quiser que seu tema seja apresentado em apenas um esquema de cores, você vai precisar especificar isto explicitamente no `package.json`:

```json
// package.json
{
  "name": "slidev-theme-meu-tema-legal",
  "keywords": [
    "slidev-theme",
    "slidev"
  ],
  "slidev": {
    "colorSchema": "light" // ou "dark" ou "both"
  }
}
```

Para acessar o modo escuro quando estiver criando os estilos do seu tema, você pode usar a especificação de modo escuro do CSS com a classe `dark`:

```css
/* css generalista aqui */

html:not(.dark) {
  /* css para modo claro aqui */
}

html.dark {
  /* css para modo escuro aqui */
}
```

O Slidev alterna a classe `dark` no elemento `html` da página para trocar o esquema de cores.

### Destacador

As cores do destacador de sintaxe também são provenientes do tema. Nós suportamos tanto o [Prism](https://prismjs.com/) quanto o [Shiki](https://github.com/shikijs/shiki). Para mais informação consulte a [documentação do destaque de sintaxe](custom/highlighters).

Você pode suportar um deles, ou os dois. Consulte os exemplos de configuração padrão de tema [`./styles/prism.css`](https://github.com/slidevjs/slidev/blob/main/packages/theme-default/styles/prism.css) / [`./setup/shiki.ts`](https://github.com/slidevjs/slidev/blob/main/packages/theme-default/setup/shiki.ts).

Além disso, lembre-se de especificar os destacadores suportados no seu `package.json`:

```json
// package.json
{
  "slidev": {
    "highlighter": "shiki" // ou "prism" ou "all"
  }
}
```

### Versão do Slidev

Se o tema estiver usando um recurso específico do Slidev que foi introduzido recentemente, você pode definir a versão mínima do Slidev necessária para ter o seu tema funcionando bem:

```json
// package.json
{
  "engines": {
    "slidev": ">=0.19.3"
  }
}
```

Se usuário estiverem usando versões mais antigas do Slidev, será gerado um erro.
<span style='float: footnote;'><a href="../index.html#toc">Go to TOC</a></span>
