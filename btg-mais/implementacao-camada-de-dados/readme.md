![Reamp](https://github.com/adtechReamp/client/blob/main/logo.png?raw=true)

> Adtech<br />
> Documento de Especificação Técnica

<br />

## Implementação da Camada de dados - BTG Mais
Última atualização: 17/12/2020 <br />
Em caso de dúvidas, entrar em contato com: [tag@reamp.com.br](tag@reamp.com.br)

<br />

## Sumário

- [Objetivo](#objetivo)
- [Implementação](#implementa%c3%a7%c3%a3o)
- [Especificações Globais](#especifica%c3%a7%c3%b5es-globais)
- [Dimensões Globais](#dimens&otilde;es-globais)
- [Geral](#geral)


## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [https://www.btgmais.com/](https://www.btgmais.com/).

<br />

### **Descrição Geral**

O `snippet` do Google Tag Manager é um pequeno trecho de código javascript ou non-javascript, através do uso de um iframe quando o javascript não está disponível, que é inserido nas páginas do site, tornando possível que a configuração das tags sejam realizadas via interface.


### **Posicionamento do Código - Google Tag Manager**

#### 1. Copie o seguinte JavaScript e cole-o o mais próximo da tag `<head>` de abertura possível em todas as páginas do seu site.

```html

<html>
  <head>
    <!-- Google Tag Manager -->
    <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start': new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0], j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src='https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f); })(window,document,'script','dataLayer','GTM-TK8RPDC');</script>
    <!-- End Google Tag Manager -->
    //...
  </head>
```

#### 2. Copie o seguinte trecho e cole-o imediatamente após a marcação `<body>` de abertura em cada página do seu site.

```html
<body>
  <!-- Google Tag Manager (noscript) -->
  <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-GTM-TK8RPDC" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
  <!-- End Google Tag Manager (noscript) -->
  //...
  </body>
</html>
```

Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)


## Observações
> Os valores especificados entre colchetes `[[ ]]` são variáveis dinâmicas e devem ser substituídas por seus respectivos valores.<br />

> Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais. <br />

> Caso o site já possua o Google Analytics instalado, será necessário a remoção do código de **todas as páginas do site**: <br />

```html

<script>
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o), m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)})(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

ga('create', 'UA-XXXXXXXX-X', 'auto');
ga('send', 'pageview');
</script>

```

---

```html

<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-XXXXXXX-X"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-XXXXXXX-X');
</script>
```

---

## Implementação

### Camada de dados (DataLayer)

> É um array de objetos javascript utilizado pelo Google Tag Manager para receber em seus atributos, dados importantes do site.
Para implementar o dataLayer no site, o desenvolvedor pode utilizar formas diferentes para preencher os dados. Essas formas são dependentes da ação estabelecida na documentação e também do nível da interação.

**Instalação**<br />
Inserir a camada de dados antes do snippet de instalação do Google Tag Manager. Exemplo:


```html
<script>
	window.dataLayer = window.dataLayer || [];
	window.dataLayer.push({
		'atributo1': '[[valor1]]',
		'atributo2': '[[valor2]]'
	});
</script>
```

### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponível retornar tipagem ´undefined´.



### Dimensões Globais:

**Dimensões Customizadas para todas as páginas:**<br />
Deve ser disparado um push de dataLayer no momento de carregamento de todas as páginas do site (Considerar também todas as trocas de Path, quando o conteúdo da página é alterado mas a página não recarrega).<br />

```html
<script>
	window.dataLayer = window.dataLayer || [];
	dataLayer.push({
		'dimension1': '[[User ID]]',
		'dimension2': '[[GA ClientID]]',
		'dimension3': '[[GTM-ID]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[User ID]]			| '1234656'			    | ID definido após o envio com sucesso do formulário										|
| [[GA ClientID]]		| 'XXXXXXXXXX:XXXX:XX'	| ID aleatório do Google Analytics										|
| [[GTM-ID]]			| 'GTM-TK8RPDC'		| ID do container do GTM instalado no ambiente										|

---

### Geral

**Quando: No clique dos itens do header**<br />

- **Onde:** Em  todas as páginas que estiver disponível
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'genericEvent',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'clique:header',
    'eventLabel': '[[nome-item]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[nome-item]] | 'nossos-produtos', 'logo', 'seus-beneficios', 'entre-na-fila-de-espera' e etc | Retornar o nome do item clicado.   |


<br />

**Quando: No clique do menu superior**<br />

- **Onde:** Em  todas as páginas que estiver disponível
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'genericEvent',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'clique:menu',
    'eventLabel': '[[nome-menu]]:[[submenu]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[nome-menu]] | nossos-produtos' e etc |  Retornar o nome do menu clicado.  |
| [[submenu]] | 'cartao-de-credito', 'conta-corrente' e etc | Retornar o nome do submenu clicado.   |


<br />

**Quando: No clique do itens do footer**<br />

- **Onde:** Em  todas as páginas que estiver disponível
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'genericEvent',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'clique:footer',
    'eventLabel': '[[secao]]:[[nome-item]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[secao]] | 'mapa-do-site', 'baixe-nosso-app', 'links-rapidos' e etc |  Retornar o nome da seção.  |
| [[nome-item]] | 'nossos-produtos', 'google-play', 'facebook' e etc |  Retornar o nome do item clicado dentro da seção.  |


<br />

**Quando: Na interação com os campos do formulário**<br />

- **Onde:** Em  todas as páginas que estiver disponível.
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'genericEvent',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'interacao:formulario:[[nome-formulario]]',
    'eventLabel': 'preencheu:campo:[[nome-campo]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[nome-formulario]] | 'entre-na-lista-de-espera-btg+' e etc|  Deve retornar o nome do formulário.   |
| [[nome-campo]] | 'nome-completo', 'email' e etc |   Retornar o nome do campo preenchido.  |


<br />

**Quando: Na interação com os checkbox do formulário**<br />

- **Onde:** Em  todas as páginas que estiver disponível
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'genericEvent',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'interacao:checkbox:formulario:[[nome-formulario]]',
    'eventLabel': '[[nome-checkbox]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[nome-formulario]] | 'entre-na-lista-de-espera-btg+' e etc|  Deve retornar o nome do formulário.   |
| [[nome-checkbox]] |  'li-e-concordo-com-a-politica', 'estou-de-acordo' e etc |   Retornar o nome do checkbox que teve interação. |


<br />

**Quando: No clique dos botões do formulário**<br />

- **Onde:** Em  todas as páginas que estiver disponível
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'genericEvent',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'clique:botao:formulario:[[nome-formulario]]',
    'eventLabel': '[[nome-botao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[nome-formulario]] | 'entre-na-lista-de-espera-btg+' e etc|  Deve retornar o nome do formulário.   |
| [[nome-botao]] |  'entrar-na-lista-de-espera', 'fechar' e etc |  Retornar o nome do botão clicado. |


<br />


**Quando: Na tentativa de callback de envio do formulário**<br />

- **Onde:** Em  todas as páginas que estiver disponível
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'conversion',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'callback:formulario:[[nome-formulario]]',
    'eventLabel': '[[status]]',
    'UserID': '[[User ID]]
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[nome-formulario]] | 'entre-na-lista-de-espera-btg+' e etc|  Deve retornar o nome do formulário.   |
| [[status]] |   'sucesso', 'erro:email-invalido', 'pagina-indisponivel-no-momento' e etc |  Retornar a mensagem de sucesso ou tipo de erro. |
| [[User ID]]			| '1234656'			    | ID definido após o envio com sucesso do formulário										|

<br />

**Quando: Na interação com as dúvidas**<br />

- **Onde:** Em  todas as páginas que estiver disponível
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'genericEvent',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'interacao:duvidas',
    'eventLabel': '[[nome-pergunta]]:[[acao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[nome-pergunta]] | 'o-que-e-o-btg+', 'qual-a-diferenca-do-btg+' e etc|  Deve retornar o nome da pergunta.  |
| [[acao]] |   'abriu' ou 'fechou'  |  Deve retornar a ação do usuário. |

<br />

**Quando: No clique dos botões dos banners**<br />

- **Onde:** Em  todas as páginas que estiver disponível
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'genericEvent',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'clique:botao:banner',
    'eventLabel': '[[nome-botao]]:[[nome-banner]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[nome-botao]] | 'saiba-como-funciona' e etc | Retornar o nome do botão clicado.  |
| [[nome-banner]] | 'pix', 'chegou-o-btg-mais' e etc  |  Retornar o nome do botão clicado. |

<br />

**Quando: No clique dos botões ou links de cada seção**<br />

- **Onde:** Em  todas as páginas que estiver disponível
    
```html
<script>
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    'event': 'genericEvent',
    'eventCategory': 'btgmais:[[pagina]]',
    'eventAction': 'clique:[[botao ou link]]',
    'eventLabel': '[[nome-item]]:[[nome-secao]]'
  });
</script>
```

| Variável        | Exemplo                               | Descrição                         |
| :-------------- | :------------------------------------ | :-------------------------------- |
| [[pagina]] | 'home', 'cartao-de-credito', 'beneficios', 'entre-na-lista-de-espera' e etc | Retornar o nome da página ou nome do item em que aparece.  |
| [[botao ou link]] | 'botao' ou 'link' | Retornar o nome do botão ou link clicado.  |
| [[nome-item]] | 'faca-sua-reserva', 'tabela-de-tarifas' e etc  |   Retornar o nome do item clicado. |
| [[nome-secao]] | 'tabela-de-comparativos', 'cartao-btg+' e etc  |  Retornar o nome seção.  |

<br />


> Em caso de dúvidas, entrar em contato com: [tag@reamp.com.br](tag@reamp.com.br)

<br />

<script> document.querySelector('h1').style.display = 'none' </script>
