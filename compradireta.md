> Digital Analytics - Whirlpool BR<br />
> Documento de Especificação Técnica - Compra Direta

<br />

## Implementação da Camada de dados
Última atualização: 05/04/2022<br />
Responsável pela documentação: Caio H Silva: <br />
Em caso de dúvidas, entrar em contato com: [mdl-chapter_cro@whirlpool.com](mailto:mdl-chapter_cro@whirlpool.com)

<br />

## Índice
- [Objetivo](#objetivo)
- [Overview e Descrições Técnicas](#overview-e-descrições-técnicas)
- [Especificação de Micro-conversões](#especificação-de-micro-conversões)
- [Especificação de Conversões](#especificação-de-conversões)
- [Considerações Finais](#considerações-finais)

<br />

## Objetivo
Este documento tem como objetivo instruir a implementação do Google Tag Manager, da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de [Compra Direta](https://www.compradiretaempresas.com.br/).

<br />

## Overview e Descrições Técnicas

### Instalação do Google Tag Manager

> Para instalar o Google Tag Manager no seu site, basta seguir os seguintes passos: 

**Dentro do "head"**<br />
Cole esse código o mais alto possível na tag *head* da página:

```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-N486HMV');</script>
<!-- End Google Tag Manager -->
```
<br />

**Início do "body"**<br />
Além disso, cole esse código imediatamente após a tag de abertura *body*:

```html
<!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-N486HMV"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->
```

<br />

### Camada de dados (DataLayer)

> É um array de objetos javascript utilizado pelo Google Tag Manager para receber em seus atributos, dados importantes do site.
Para implementar o dataLayer no site, o desenvolvedor pode utilizar formas diferentes para preencher os dados. Essas formas são dependentes da ação estabelecida na documentação e também do nível da interação.

**Instalação**<br />
Inserir a camada de dados antes do snippet de instalação do Google Tag Manager. Exemplo:

```html
<script>
	window.dataLayer = window.dataLayer || [];
	window.dataLayer = [{
		'atributo1': '[[valor1]]',
		'atributo2': '[[valor2]]'
	}];
</script>
```

OU

```html
<script>
	window.dataLayer = window.dataLayer || [];
	window.dataLayer.push({
		'atributo1': '[[valor1]]',
		'atributo2': '[[valor2]]'
	});
</script>
```

<br />	 	

## Implementação

A documentação foi descrita para algumas áreas especificas do ambiente [Compra Direta](https://www.compradiretaempresas.com.br/).


### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponível retornar 'nao_disponivel'. - Mudar de acordo com o projeto

---

<br />

## Ecommerce GA4:

### Product Impression:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento de uma página de lista de produtos.

```html
<script>
dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
dataLayer.push({
  event: "view_item_list",
  ecommerce: {
    items: [
     {
       item_name: "Geladeira Frost Free Inverse",       // Name or ID is required.
       item_id: "bro80ak",
       price: 6009.00,
       item_brand: "Brastemp",
       item_category: "Geladeira",
       item_category2: "Frost Free",
       item_category3: "Inverse",
       item_category4: "Inox",
       item_variant: "110v",
       item_list_name: "Refrigerador",
       item_list_id: "BTP1234",
       index: 1,
       quantity: 1
     },
     {
       item_name: "Geladeira Frost Free Duplex",
       item_id: "brm54hk",
       price: 3769.00,
       item_brand: "Brastemp",
       item_category: "Geladeira",
       item_category2: "Frost Free",
       item_category3: "Duplex",
       item_category4: "Inox",
       item_variant: "110v",
       item_list_name: "Refrigerador",
       item_list_id: "BTP456",
       index: 2,
       quantity: 1
     }]
  }
});
</script>
```

<br />

| Nome Variável 		| Exemplo 					| Descrição 							|
|:---------------------	| :------------------------ | :------------------------------------	|
| item_name 			| 'Geladeira Frost Free' 	| Nome do produto 						|
| item_id 				| 'bro80ak'					| ID do produto 						|
| price 				| '6009.00' 				| Preço do produto						|
| item_brand 			| 'Brastemp'				| Marca do produto 	 					|
| item_category 		| 'Geladeira'				| Categoria do produto 					|
| item_category2 		| 'Frost Free'				| Sub-Categoria do produto 				|
| item_category3 		| 'Inverse', 'Duplex'		| Sub-Categoria do produto 				|
| item_variant 			| '110v'					| Variação do produto (voltagem)		|
| item_list_name		| 'Refrigerador' 			| Nome da lista de produtos 			|
| item_list_id 			| 'BTP1234' 				| ID da Lista de Produtos 				|
| index					| '1', '2' 					| Posição do produto na lista			|
| quantity 				| '1'						| Quantidade Produtos 					|

<br />

### Product Click:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no clique em algum produto.

```html
<script>
function onProductClick(productObj) {
  dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
  dataLayer.push({
    event: "select_item",
    ecommerce: {
      items: [{
        item_name: productObj.name, // Name or ID is required.
        item_id: productObj.id,
        item_brand: productObj.brand,
        item_category: productObj.category,
        item_category2: productObj.category_2,
        item_category3: productObj.category_3,
        item_category4: productObj.category_4,
        item_variant: productObj.variant,
        item_list_name: productObj.list_name,
        item_list_id: productObj.list_id,
        index: productObj.index,
        quantity: productObj.quantity,
        price: productObj.price
      }]
    }
  });
}
</script>
```

<br />

| Nome Variável 		| Exemplo 					| Descrição 							|
|:---------------------	| :------------------------ | :------------------------------------	|
| item_name 			| 'Geladeira Frost Free' 	| Nome do produto 						|
| item_id 				| 'bro80ak'					| ID do produto 						|
| price 				| '6009.00' 				| Preço do produto						|
| item_brand 			| 'Brastemp'				| Marca do produto 	 					|
| item_category 		| 'Geladeira'				| Categoria do produto 					|
| item_category2 		| 'Frost Free'				| Sub-Categoria do produto 				|
| item_category3 		| 'Inverse', 'Duplex'		| Sub-Categoria do produto 				|
| item_variant 			| '110v'					| Variação do produto (voltagem)		|
| item_list_name		| 'Geladeiras' 				| Nome da lista de produtos 			|
| item_list_id 			| 'BTP1234' 				| ID da Lista de Produtos 				|
| index					| '1' 						| Posição do produto na lista			|
| quantity 				| '1'						| Quantidade Produtos 					|

<br />

### Product Detail:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento da página de um produto.

```html
<script>
dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
dataLayer.push({
  event: "view_item",
  ecommerce: {
    items: [{
      item_name: "Geladeira  Frost Free Side Inverse", // Name or ID is required.
      item_id: "bro80ak",
      price: 6009.00,
      item_brand: "Brastemp",
      item_category: "Geladeira",
      item_category2: "Frost Free",
      item_category3: "Inox"
      item_variant: "110v",
      item_list_name: "Refrigerador",  // If associated with a list selection.
      item_list_id: "BRP1234",  // If associated with a list selection.
      index: 1,  // If associated with a list selection.
      quantity: 1
    }]
  }
});
</script>
```

<br />

| Nome Variável 		| Exemplo 					| Descrição 							|
|:---------------------	| :------------------------ | :------------------------------------	|
| item_name 			| 'Geladeira Frost Free' 	| Nome do produto 						|
| item_id 				| 'bro80ak'					| ID do produto 						|
| price 				| '6009.00' 				| Preço do produto						|
| item_brand 			| 'Brastemp'				| Marca do produto 	 					|
| item_category 		| 'Geladeira'				| Categoria do produto 					|
| item_category2 		| 'Frost Free'				| Sub-Categoria do produto 				|
| item_category3 		| 'Inverse', 'Duplex'		| Sub-Categoria do produto 				|
| item_variant 			| '110v'					| Variação do produto (voltagem)		|
| item_list_name		| 'Geladeiras' 				| Nome da lista de produtos 			|
| item_list_id 			| 'BTP1234' 				| ID da Lista de Produtos 				|
| index					| '1' 						| Posição do produto na lista			|
| quantity 				| '1'						| Quantidade Produtos 					|

<br />

### AddToCart:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no clique do CTA de adição de produto ao carrinho.

```html
<script>
dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
dataLayer.push({
  event: "add_to_cart",
  ecommerce: {
    items: [{
      item_name: "Geladeira  Frost Free Side Inverse", // Name or ID is required.
      item_id: "bro80ak",
      price: 6009.00,
      item_brand: "Brastemp",
      item_category: "Geladeira",
      item_category2: "Frost Free",
      item_category3: "Inverse",
      item_category4: "Inox",
      item_variant: "110v",
      item_list_name: "Refrigerador",
      item_list_id: "BTP1234",
      index: 1,
      quantity: 2
    }]
  }
});
</script>
```

<br />

| Nome Variável 		| Exemplo 					| Descrição 							|
|:---------------------	| :------------------------ | :------------------------------------	|
| item_name 			| 'Geladeira Frost Free' 	| Nome do produto 						|
| item_id 				| 'bro80ak'					| ID do produto 						|
| price 				| '6009.00' 				| Preço do produto						|
| item_brand 			| 'Brastemp'				| Marca do produto 	 					|
| item_category 		| 'Geladeira'				| Categoria do produto 					|
| item_category2 		| 'Frost Free'				| Sub-Categoria do produto 				|
| item_category3 		| 'Inverse', 'Duplex'		| Sub-Categoria do produto 				|
| item_variant 			| '110v'					| Variação do produto (voltagem)		|
| item_list_name		| 'Geladeiras' 				| Nome da lista de produtos 			|
| item_list_id 			| 'BTP1234' 				| ID da Lista de Produtos 				|
| index					| '1' 						| Posição do produto na lista			|
| quantity 				| '1'						| Quantidade Produtos 					|

<br />

### Remove From Cart:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no clique do botão para remover um produto do carrinho.

```html
<script>
dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
dataLayer.push({
  event: "remove_from_cart",
  ecommerce: {
    items: [{
      item_name: "Geladeira  Frost Free Side Inverse", // Name or ID is required.
      item_id: "bro80ak",
      price: 6009.00,
      item_brand: "Brastemp",
      item_category: "Geladeira",
      item_variant: "110v",
      item_list_name: "Refrigerador",  // If associated with a list selection.
      item_list_id: "BTP1234",  // If associated with a list selection.
      index: 1,  // If associated with a list selection.
      quantity: 1
    }]
  }
});
</script>
```

<br />

| Nome Variável 		| Exemplo 					| Descrição 							|
|:---------------------	| :------------------------ | :------------------------------------	|
| item_name 			| 'Geladeira Frost Free' 	| Nome do produto 						|
| item_id 				| 'bro80ak'					| ID do produto 						|
| price 				| '6009.00' 				| Preço do produto						|
| item_brand 			| 'Brastemp'				| Marca do produto 	 					|
| item_category 		| 'Geladeira'				| Categoria do produto 					|
| item_category2 		| 'Frost Free'				| Sub-Categoria do produto 				|
| item_category3 		| 'Inverse', 'Duplex'		| Sub-Categoria do produto 				|
| item_variant 			| '110v'					| Variação do produto (voltagem)		|
| item_list_name		| 'Geladeiras' 				| Nome da lista de produtos 			|
| item_list_id 			| 'BTP1234' 				| ID da Lista de Produtos 				|
| index					| '1' 						| Posição do produto na lista			|
| quantity 				| '1'						| Quantidade Produtos 					|

<br />

### Promotion Impression:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez na visualização de um banner.

```html
<script>
dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
dataLayer.push({
  event: "view_promotion",
  ecommerce: {
    items: [{
      item_name: "Geladeira  Frost Free Side Inverse", // Name or ID is required.
      item_id: "bro80ak",
      price: 6009.00,
      item_brand: "Brastemp",
      item_category: "Geladeira",
      item_category2: "Frost Free",
      item_category3: "Inverse",
      item_category4: "Inox",
      item_variant: "110v",
      promotion_id: "abc123",
      promotion_name: "summer_promo",
      creative_name: "instore_suummer",
      creative_slot: "1",
      location_id: "hero_banner",
      index: 1,
      quantity: 1
    }]
  }
});
</script>
```

<br />

### Promotion Click:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no clique de uma promoção.

```html
<script>
function onPromoClick(promoObj) {
  dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
  dataLayer.push({
    event: "select_promotion",
    ecommerce: {
      items: [{
        item_name: promoObj.name, // Name or ID is required.
        item_id: promoObj.id,
        item_brand: promoObj.brand,
        item_category: promoObj.category,
        item_category2: productObj.category_2,
        item_category3: productObj.category_3,
        item_category4: productObj.category_4,
        item_variant: promoObj.variant,
        promotion_id: promoObj.pid,
        promotion_name: promoObj.pname,
        creative_name: promoObj.pcreative_name,
        creative_slot: promoObj.pcreative_slot,
        location_id: promoObj.plocation,
        index: promoObj.index,
        quantity: promoObj.quantity,
        price: promoObj.price
      }]
    }
  });
}
</script>
```

<br />

---

### Especificação de Conversões:

**Sucesso Compra:**<br />

- **Onde:** No callback de sucesso;

```html
<script>
	dataLayer.push({
		'event': 'conversion',
		'eventCategory': 'ecommerce',
		'eventAction': 'sucesso:compra'
	});
</script>
```

<br />

### Checkout:

****<br />

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento da páginas página de checkout.

```html
<script>
function onCheckout() {
  dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
  dataLayer.push({
    event: "begin_checkout",
    ecommerce: {
      items: [{
        item_name: "Geladeira  Frost Free Side Inverse", // Name or ID is required.
        item_id: "bro80ak",
        price: 6009.00,
        item_brand: "Brastemp",
     	item_category: "Geladeira",
     	item_category2: "Frost Free",
     	item_category3: "Inverse",
     	item_category4: "Inox",
     	item_variant: "110v",
     	item_list_name: "Refrigerador",
     	item_list_id: "BTP1234",
        index: 1,
        quantity: 1
      }]
    }
  });
}
</script>
```

<br />

| Nome Variável 		| Exemplo 					| Descrição 							|
|:---------------------	| :------------------------ | :------------------------------------	|
| item_name 			| 'Geladeira Frost Free' 	| Nome do produto 						|
| item_id 				| 'bro80ak'					| ID do produto 						|
| price 				| '6009.00' 				| Preço do produto						|
| item_brand 			| 'Brastemp'				| Marca do produto 	 					|
| item_category 		| 'Geladeira'				| Categoria do produto 					|
| item_category2 		| 'Frost Free'				| Sub-Categoria do produto 				|
| item_category3 		| 'Inverse', 'Duplex'		| Sub-Categoria do produto 				|
| item_variant 			| '110v'					| Variação do produto (voltagem)		|
| item_list_name		| 'Geladeiras' 				| Nome da lista de produtos 			|
| item_list_id 			| 'BTP1234' 				| ID da Lista de Produtos 				|
| index					| '1' 						| Posição do produto na lista			|
| quantity 				| '1'						| Quantidade Produtos 					|


<br />

### Purchase:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento da página de transação, e se o usuário acessar novamente o link ou atualizar a página, o objeto não deve ser disparado novamente.

```html
<script>
dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
dataLayer.push({
  event: "purchase",
  ecommerce: {
      transaction_id: "T12345",
      affiliation: "Compra Certa",
      value: "9679.00",
      tax: "104.00",
      shipping: "5.99",
      currency: "BRL",
      coupon: "SUMMER_SALE",
      items: [{
        item_name: "Geladeira  Frost Free Side Inverse",
        item_id: "bro80ak",
        price: 6009.00,
        item_brand: "Brastemp",
        item_category: "Geladeira",
        item_variant: "110v",
        quantity: 1
      }, {
        item_name: "Geladeira Frost Free Duplex",
        item_id: "brm54hk",
        price: 3679.00,
        item_brand: "Brastemp",
        item_category: "Geladeira",
        item_variant: "110v",
        quantity: 1
      }]
  }
});
</script>
```

*Descrição Purchase:*

| Nome Variável 		| Exemplo 			| Descrição 										|
| :--------------------	| :----------------	| :------------------------------------------------	|
| transaction_id 		| 'T12345' 			| ID da Transação									|
| affiliation 			| 'Compra Certa'	| Loja que ocorreu a transação 						|
| value					| '9679.00' 		| Valor total da transação incluindo frete e taxas	|
| tax 					| '104.00' 			| Taxas da transação 	 							|
| shipping 		 		| '4.50' 			| Frete da transação 								|
| currency				| 'BRL'				| Moeda 											|
| coupon				| 'SUMMER_SALE'		| Cupom de desconto usado na transação				|

<br />

*Descrição Produtos:*

| Nome 	Variável 		| Exemplo 					| Descrição 							|
|:---------------------	| :------------------------ | :------------------------------------	|
| item_name 			| 'Geladeira Frost Free' 	| Nome do produto 						|
| item_id 				| 'bro80ak' 				| ID do produto 						|
| price 				| '6009.00' 				| Preço do produto						|
| item_brand 			| 'Brastemp'				| Marca do produto 	 					|
| item_category 		| 'Geladeira'				| Categoria do produto 					|
| item_category2 		| 'Frost Free'				| Sub-Categoria do produto 				|
| item_category3 		| 'Inverse', 'Duplex'		| Sub-Categoria do produto 				|
| item_variant 			| '110v'					| Variação do produto (voltagem)		|
| quantity 				| '1'						| Quantidade Produtos 					|

<br />

### Refund:

- **Onde:** O objeto javascript (dataLayer) abaixo deve ser disparado uma única vez no carregamento da página de reembolso 

**Reembolso total**
```html
<script>
dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
dataLayer.push({
  event: "refund",
  ecommerce: {
    transaction_id: "T12345" // Transaction ID. Required for purchases and refunds.
  }
});
</script>
```


**Reembolso parcial**
```html
<script>
dataLayer.push({ ecommerce: null });  // Clear the previous ecommerce object.
dataLayer.push({
  event: "refund",
  ecommerce: {
      transaction_id: "T12345", // Transaction ID.
      items: [{
       item_name: "Geladeira  Frost Free Side Inverse",
       item_id: "bro80ak", // ID is required.
       price: 6009.00,
       item_brand: "Brastemp",
       item_category: "Geladeira",
       item_category2: "Frost Free",
       item_category3: "Inverse",
       item_category4: "Inox",
       item_variant: "110v",
       item_list_name: "Refrigerador", // If associated with a list selection.
       item_list_id: "BTP1234", // If associated with a list selection.
       index: 1, // If associated with a list selection.
       quantity: 1 // Quantity is required.
      }]
  }
});
</script>
```

<br />

## Considerações Finais:

> Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/ecommerce-ga4)
