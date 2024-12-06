#  E o Oscar vai para...?

Muito bem, Pessoal... 

Hoje vamos trabalhar com o Oscar.
A ideia de premiar ou ser premiado é para reconhecer:
- Reconhecer uma qualidade
- Reconhecer um atributo
- Reconhecer o esforço... 

Alguns diriam que prêmios são apenas um reconhecimento superficial do nosso trabalho, uma forma de massagear o ego e nos deixar com a sensação de que estamos fazendo algo certo. Outros, no entanto, defendem que os prêmios são uma forma de validação do nosso trabalho, uma prova de que estamos no caminho certo e de que estamos fazendo a diferença.

Prêmios têm sim o seu valor, mas que eles não são o único indicador de sucesso. É claro que é sempre bom ser reconhecido pelo nosso trabalho e ter um troféu ou uma placa para pendurar na parede, mas o mais importante mesmo é a satisfação pessoal que sentimos quando fazemos algo que amamos e que acreditamos ser importante. Então, sim, prêmios importam, mas não devemos deixá-los ser o único fator que determina o nosso sucesso e a nossa felicidade.

Nem todos os prêmios são merecidos e nem todos que merecem ganham prêmios. 
Então vale mesmo a pena, premiar? 

## Exercicios:

**1- Quantas Vezes Natalie Portman Foi Indicada ao Oscar?**
```js
db["registros"].countDocuments({nome_do_indicado:"Natalie Portman"})

```
Resposta: 3

**2- Quantos Oscars Natalie Portman Ganhou?**

```js
db["registros"].countDocuments({nome_do_indicado:"Natalie Portman",vencedor:"1"})

```
Resposta: 1

**3- Amy Adams já Ganhou Algum Oscar?**

```js
var existe = db.registros.find({ nome_do_indicado: "Amy Adams", vencedor: "1" }).count() > 0;

if (existe) {
    print("sim");
} else {
    print("não");
}
 ```
Resposta: Não
 
 ```js
db["registros"].countDocuments({nome_do_indicado:"Amy Adams", vencedor:"1"})
 ```
Resposta: 0


**4- A Série de Filmes Toy Story Ganhou um Oscar em Quais Anos?**
```js
db["registros"].find({nome_do_filme:/Toy Story/,vencedor:"1"}, {ano_cerimonia: 1, _id: 0})

```
  
  Resposta: ano_cerimonia: 2011
  ano_cerimonia: 2011
  ano_cerimonia: 2020

**5- A Partir de Que Ano Que a Categoria "Actress" Deixa de Existir?**
```js
db["registros"].find( { categoria: "ACTRESS", vencedor: "1" }, { ano_cerimonia: 1, _id: 0 }
).sort({ano_cerimonia: -1 }).limit(1)
```

Resposta:  ano_cerimonia: 1976

**6- O Primeiro Oscar Para Melhor Atriz Foi Para Quem, Em Que Ano?**
```js
db["registros"].find({categoria:"ACTRESS",vencedor:'1'},{ano_cerimonia:1,nome_do_indicado:1,_id:0}).sort({ano_cerimonia:1}).limit(1)

```
Resposta:  ano_cerimonia: 1928,
  nome_do_indicado: 'Janet Gaynor'

**7- Na Campo "Vencedor", Altere Todos os Valores Com "Sim" Para 1 e Todos os Valores "Não" Para 0.**
```js
db.registros.updateMany({vencedor:'false'},{$set:{vencedor:'0'}})  // vencedor:"false", alterado para 0

db.registros.updateMany({vencedor:'true'},{$set:{vencedor:'1'}})   // vencedor:"true", alterado para 1

```

**8- Em Qual Edição do Oscar "Crash" Concorreu ao Oscar?**

```js
db["registros"].find({nome_do_filme:"Crash"},{ano_cerimonia:1,_id:0}).sort({ano_cerimonia:1}).limit(1)
```
Resposta: ano_cerimonia: 2006

**9- Bom... dê um Oscar Para um Filme que Merece Muito, Mas Não Ganhou.**

Resposta: Um Dos Filmes Indicados Que Mereceu o Oscar, Mas Não o Ganhou é: Django Livre

```js
db["registros"].updateOne({ nome_do_filme: /Django Livre/, vencedor: "0" },{ $set: { vencedor: '1' } })

```

**10- O Filme Central Station Aparece no Oscar?**
```js
const filme = db.registros.findOne({ nome_do_filme: "Central Station" });

if (filme) {
  print("Sim, ano da cerimônia: " + filme.ano_cerimonia);
} else {
  print("Não");
}

```
Resposta: Sim, Ano da Cerimônia: 1999

**11- Inclua no Banco 3 Filmes que Nunca Foram Nem Nomeados ao Oscar, Mas que Merecem Ser.**


Resposta: Os filmes eu Acho que Deveriam ser indicados são: Circulo de Fogo, Círculo de Fogo: A Revolta, Gigantes de Aço.

```js
db.registros.insertMany([
    { nome_do_filme: "Circulo de Fogo", categoria: "ficção científica", vencedor: '1' },
    { nome_do_filme: "Circulo de Fogo: A Revolta", categoria: "ficção científica", vencedor: '1' },
    { nome_do_filme: "Gigantes de Aço", categoria: "Ação", vencedor: '1' }
])

```
**12 - Pensando no Ano em Que você Nasceu: Qual foi o Oscar de Melhor Filme, Melhor Atriz e Melhor Diretor?**
```js
db.registros.find({ ano_cerimonia: 2005, vencedor: '1' }, 
  { nome_do_filme: 1, nome_do_indicado: 1, categoria: 1, _id: 0 })

  db.registros.find(
  { 
    ano_cerimonia: 2005, 
    vencedor: '1', 
    $or: [
      { categoria: /ACTRESS/ },
      { categoria: "DIRECTING" }
    ]
  }, 
  { 
    nome_do_filme: 1, 
    nome_do_indicado: 1, 
    categoria: 1, 
    _id: 0 
  }
)

``` 
Melhor Filme: The Aviator
  
  ano_cerimonia: 2005,
  categoria: 'ACTRESS IN A SUPPORTING ROLE',
  nome_do_indicado: 'Cate Blanchett',
  nome_do_filme: 'The Aviator'

Melhor Atriz: Cate Blanchett

Melhor Diretor: Em 2005 Somente o Diretor "Clint Eastwood" Ganhou o Oscar na Categoria Direção
 
 ano_cerimonia: 2005,
  categoria: 'DIRECTING',
  nome_do_indicado: 'Clint Eastwood',
  nome_do_filme: 'Million Dollar Baby'
