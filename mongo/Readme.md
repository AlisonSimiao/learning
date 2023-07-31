# Mongo

O MongoDB é um banco de dados NoSQL orientado a documentos, conhecido por sua flexibilidade, escalabilidade e alta performance. Ele armazena dados em documentos BSON dentro de coleções, permitindo que cada documento tenha uma estrutura diferente. Sua capacidade de escalonamento horizontal o torna ideal para aplicações com grande volume de dados. O MongoDB é amplamente utilizado em projetos web e móveis, análise de big data e outras aplicações que requerem alta disponibilidade e tolerância a falhas.

[Documentação oficial](https://www.mongodb.com/docs/)

## Execução
>
> docker exec -it < nome do container> mongo --port < porta > -u < usuario > -p < senha > --authenticationDatabase < nome do banco>

ou

> mongod

## Comandos

Comando | Descrição
--- | ---
use < nome do banco >| selecionar um banco do mongo, no caso de estar vazio ele sera criado quando houver a primeira inserção de dado
show dbs | mostra todos os bancos de dados ja criados e com pelo menos um dado
show collections | mostra todas as coleções(tabelas, em SQL) presentes no banco corrente
db | passa a se referir ao banco de dados current e suas funções sao acessadas como um [objeto js](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Working_with_objects)

## Crud (Create/Read/Update/Delete)

### Create

#### `insertOne` - um dado

Cria na coleção `pessoas` um dado com `nome = "test"` e `idade = 456`
> db.pessoas.insertOne(
> {nome: "test", idade: 456}
> )

retorna se deu tud certo e um Id unico daquele dado

```json
{
    "acknowledged" : true,
    "insertedId" : ObjectId("64c7ad00100c413c732b6358")
 }
```

#### `insertMany` - muitos dados

criar na coleção pessoas dois dados dentro do array

```bash
[
    {nome: "test", idade: 456}, 
    {nome: "test 2", idade: 555}
]
```

> db.pessoas.insertMany(
> [
>
> {nome: "test", idade: 456},
>
> {nome: "test 2", idade: 555}
>
> ])

```bash
{
 "acknowledged" : true,
 "insertedIds" : [
  ObjectId("64c7aef46d0a35dff5018c94"),
  ObjectId("64c7aef46d0a35dff5018c95")
 ]
}
```

### Find

- busca todos os dados da coleção pessoas
  > db.pessoas.find()

- busca todos os dados com idade igual a 555
    > db.pessoas.find({idade: 555})

- busca o primeiro dado
    > db.pessoas.findOne()

- conta os dados encontrados
    > db.pessoas.find().count()
    >
    > 2

### Update

#### `updateOne(busca, novosValores)` - atualiza um dado

- atualiza um dado em pessoas, que tenha `idade = 555`, para ter o `nome = "idade mudada"`
    > db.pessoas.updateOne({idade: 555}, { $set: { nome: "idade mudada" } })

    ```json
    { 
        "acknowledged" : true, 
        "matchedCount" : 1, 
        "modifiedCount" : 1 
    }
    ```

#### `updateMany(busca, novosValores)` - atualiza varios dados

- atualiza todos dado em pessoas, que tenha `idade = 18`, para ter o `nome = "idade mudada"`
    > db.pessoas.updateMany({idade: 18}, { $set: { nome: "idade mudada" } })

    ```json
    { 
        "acknowledged" : true, 
        "matchedCount" : 10, 
        "modifiedCount" : 10 
    }
    ```

### Delete

#### `deleteOne(busca)` - deleta um dado

- deleta um dado em pessoas, que tenha `idade = 20`
    > db.pessoas.deleteOne({idade: 18})

    ```json
    { 
        "acknowledged" : true, 
        "deletedCount" : 1
    }
    ```

#### `deleteMany(busca)` - deleta varios dado

- deleta um dado em pessoas, que tenha `idade = 20`
    > db.pessoas.deleteOne({idade: 18})

    ```json
    { 
        "acknowledged" : true, 
        "deletedCount" : 10
    }
    ```

# Apendice

## Glossario

`acknowledged`: É um campo booleano que indica se a operação foi reconhecida pelo servidor do MongoDB. Se o valor for `true`, significa que a operação foi confirmada com sucesso.

`matchedCount`: Este campo indica o número de documentos que corresponderam aos critérios de filtro da operação realizada. Neste caso, a operação encontrou e correspondeu a 1 documento.

`modifiedCount`: É o número de documentos que foram efetivamente modificados pela operação. Neste caso, 1 documento foi modificado como resultado da operação.

## Operadores

> db.pessoas.find({idade: {$gt: 456} })

operador | descrição
--- | ---
$eq |Igual a um valor específico.
$ne |Diferente de um valor específico.
$gt |Maior que um valor específico.
$gte| Maior ou igual a um valor específico.
$lt |Menor que um valor específico.
$lte | Menor ou igual a um valor específico.
$and | Realiza a operação lógica AND em várias condições.
$or | Realiza a operação lógica OR em várias condições.
$not | Nega uma expressão lógica.
$exists | Verifica se um campo existe ou não em um documento.
$type | Verifica se um campo é de um tipo de dado específico.
$project | Controla quais campos incluir ou excluir nos resultados da consulta.
$in | Verifica se um valor está presente em uma matriz.
$nin | Verifica se um valor não está presente em uma matriz.
$all | Verifica se todos os valores fornecidos estão presentes em uma matriz.
$elemMatch | Realiza uma consulta em uma matriz de documentos e retorna o primeiro documento que corresponde aos critérios especificados.
$set | Atualiza o valor de um campo específico.
$unset | Remove um campo de um documento.
$inc | Incrementa o valor numérico de um campo.
$push | Adiciona um elemento a uma matriz.
$pull | Remove elementos de uma matriz que atendam a determinados critérios.
$addToSet | Adiciona um elemento a uma matriz, apenas se ele ainda não existir.
$pop | Remove o primeiro ou o último elemento de uma matriz.

## Tipos de dados

| Tipo de Dado   | Descrição| Exemplo|
|---|------|---|
| `double`       | Número de ponto flutuante de 64 bits.                                        | `3.14159`                     |
| `string`       | Texto Unicode.                                                              | `"Olá, mundo!"`               |
| `bool`         | Valor booleano (verdadeiro/falso).                                          | `true` ou `false`             |
| `int`          | Número inteiro de 32 bits.                                                  | `42`                          |
| `long`         | Número inteiro de 64 bits.                                                  | `1234567890`                  |
| `decimal`      | Número decimal de alta precisão.                                            | `3.14159265358979323846`      |
| `date`         | Data e hora, representada como um timestamp Unix de 64 bits.                | `ISODate("2023-07-31T12:34")` |
| `timestamp`    | Um timestamp específico do MongoDB, com informações de data e hora.         | `Timestamp(1627748040, 1)`    |
| `object`       | Documento incorporado ou subdocumento.                                      | `{ "nome": "João", "idade": 30 }`  |
| `array`        | Uma matriz ou lista de valores.                                             | `[1, 2, 3, 4, 5]`             |
| `null`         | Valor nulo ou ausente.                                                      | `null`                        |
| `objectId`     | Identificador exclusivo de um documento, geralmente gerado automaticamente. | `ObjectId("60f8794171b60c934ccd0f5e")` |
| `regex`        | Expressão regular.                                                          | `/^hello/i`                   |
| `binary`       | Dados binários (por exemplo, imagens ou arquivos).                          | `BinData(0, "SGVsbG8gV29ybGQh")` |
| `javascript`   | Código JavaScript ou função.                                                | `function() { return 42; }`   |
| `symbol`       | Símbolo usado principalmente para chaves de ordenação.                       | `Symbol("foo")`               |
| `minKey`       | Valor mínimo possível para uma chave.                                       | `MinKey`                      |
| `maxKey`       | Valor máximo possível para uma chave.                                       | `MaxKey`                      |
