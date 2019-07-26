:warning: **Infelizmente, os "problemas conhecidos" podem inviabilizar essa solução para maioria das pessoas.**

# Introdução

Se você usa um teclado padrão americano no Linux e digita em português brasileiro e inglês, provalmente já encontrou os seguintes problemas:

1. `'` + `c` vira um `ć` se você usa Inglês como idioma padrão do sistema
2. Quando digita `'` + uma letra que não aceita acento em português, como um `t`, nenhum carácter é inserido - ou pior, em algums casos, como o da letra `n`, a letra fica acentuada: `ń`. Este tipo de comportamento é muito ruim quando se está digitando em inglês: `doesn't`, por exemplo.

Infelizmente, até onde eu pude investigar, parece não ser possível simplemente criar um novo layout do tipo "pt-BR - US", que seria uma solução mais elegante. É preciso utilizar configurações customizadas do `XCompose` com `uim`.

Aplicando essas configurações, o comportamento passar a ser o seguinte:

1. `'` + `c/C`, vira  `ç/Ç` 
2. `'` + carácter que não aceita acento, vira apóstrofe + o próprio carácter. Exemplo: `'t`, `'n`, `'[`, `'}`, etc. Isso vale para crase e til também.

Este é o mesmo comportamento que existe no macOS e Windows 10 quando você seleciona o layout US international para português.

# Instalação

**Isso só foi testado no Ubuntu 19.04 com Gnome**

1. Configure o layout do teclado como `us-intl` - "English (US, intl., with dead keys)" 
2. Instale o pactote `uim`. Isso é necessário para que o XCompose possa expandir para mais de um carácter (`'t`, por exemplo). No Ubuntu/Debian, faça:

```
sudo apt update
sudo apt install uim
```

3. Configure GTK e QT para usarem o `uim`

Adicione as seguintes linhas ao seu `~/.profile`

```
export GTK_IM_MODULE="uim"
export QT_IM_MODULE="uim"
```

4. Copie o arquivo `XCompose` deste repositório para o seu `~/.XCompose`

```
curl -o ~/.XCompose https://raw.githubusercontent.com/lucasfais/xcompose-pt-BR_US/master/XCompose
```

5. Reinicie 

# Problemas conhecidos

1. Quando digitar `"` + espaço, tome o cuidado de não continuar segurando o Shift enquanto pressiona o espaço. O `uim` vem configurado para mudar o método de input quando Shift + espaço é pressionado. Infelizmente, não é possível alterar isso por causa de um bug: https://bugs.launchpad.net/ubuntu/+source/uim/+bug/1202038

    Caso pressionar Shift + espaço acidentamente, pressione novamente para voltar ao modo normal.

2. Não funciona no Slack. Provalmente porque Slack não é GTK nem QT.

# Fontes

https://wiki.debian.org/XCompose

https://unix.stackexchange.com/questions/220510/insert-both-characters-if-a-dead-key-combination-is-not-recognized-e-g-a-%E2%86%92-%C3%A1
