# Usar um Tema

Mudar o tema no Slidev é surpreendentemente fácil. Tudo que você precisa para fazer isso é adicional o campo `theme:` no seu frontmatter.

```yaml
---
theme: seriph
---
```

Você pode iniciar o servidor, que vai te solicitará instalar o tema automaticamente.

<div class="language-md">
<pre>
<span class="token keyword">?</span> The theme <span class="token string">"@slidev/theme-seriph"</span> was not found in your project, do you want to install it now? › (Y/n)
</pre>
</div>

ou instale o tema manualmente via

```bash
$ npm install @slidev/theme-seriph
```

E é só isso, aproveite o novo tema! Para mais detalhes sobre o uso, você pode consultar o README do tema.

Quer compartilhar seu tema? Aprenda sobre [como escrever um tema](themes/write-a-theme).

## Ejetar um Tema

Se você quiser ter controle total do tema atual, você pode **ejetá-lo** para os seus arquivos locais e modificá-lo como quiser. Rodando o seguinte comando:

```bash
$ slidev theme eject
```

Isto ejetará o tema que você estiver usando para `./theme`, e vai alterar o seu frontmatter para:

```yaml
---
theme: ./theme
---
```

Isto pode ser útil se você quiser fazer um tema baseado em um existente. Se fizer isso, lembre-se de mencionar o autor original do tema :)

## Tema Local

Como você provavelmente viu na seção anterior, você pode ter um tema local para os seus projetos. Tendo um **caminho relativo** no campo `theme`.

```yaml
---
theme: ./caminho/para/o/tema
---
```

Consulte [como escrever um tema](themes/write-a-theme) para mais detalhes.
<span style='float: footnote;'><a href="../index.html#toc">Go to TOC</a></span>
