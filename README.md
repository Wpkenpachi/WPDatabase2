# WPDatabase 2.0

## Instalação

#### Primeiro Passo
Abra o arquivo config. E edite os campos com as informações requisitadas:
DB_DRIVER, DB_HOST, DB_NAME, DB_USER e DB_PASS.

PS: No arquivo config, não se pode deixar espaços...(Vou consertar isso depois).
PS²: Todos os arquivos da biblioteca WPDatabase, devem ficar em um mesmo diretório.

#### Segundo Passo
Inclua o arquivo WPDatabase, onde você pretende usa-lo. Ex:
```php
    include 'WPDatabase.php';

    WPDatabase::table('table');
```
## Debug
A função debug serve para debug, mostrando a string da query que está sendo feita, sem executa-la.
Sintaxe:
```php
    debug();
```

## Custom
A função custom é a mais genérica de todas, basicamente, você pode escrever toda uma query nela, ou apenas complementar a que já está feita.
Sintaxe e Exemplo:
```php
    WPDatabase::table('table')
    ->select()
    ->custom('WHERE id = 1')
    ->debug();
    // SELECT * FROM table WHERE id = 1
```
PS: O select() é explanado logo abaixo.

## Select
Sintaxe:
```php
WPDatabase::table('table')
            ->select()
            ->debug();
```
#### Params
Os parametros podem ser passados de 2 formas.
Estes são serão os campos que serão retornados
na consulta
> select( 'campo' )
```php
WPDatabase::table('table')->select('campo1');
// SELECT campo1 FROM table
OU
WPDatabase::table('table')->select('campo1,campo2');
// SELECT campo1, campo2 FROM table

```
> select( Array $fields )
```php
$campos = [ 'table' => ['campo1', 'campo2'] ];
WPDatabase::table('table')->select($campos);
// SELECT table.campo1, table.campo2 FROM table
```

## Where
Sintaxe:
```php
WPDatabase::table('table')
            ->select()
            ->where()
            ->debug();
```
#### Params
Os parametros podem ser passados de 3 formas.
> where( ' id = 2 ' )

> where( ' id ' , 2 )

> where( ' id ' , ' = ', 2 )
```php
// SELECT * FROM table1  WHERE id = 2
```

### And
O `and()` pode ser utilizado junto ao where.
Sintaxe:
```php
WPDatabase::table('table')
            ->where('id = 2')
            ->and()
            ->custom("author = 'Wesley'")
            ->debug();
// SELECT * FROM table1  WHERE id = 2  AND author = 'Wesley'
```

### Or
O `or()` pode ser utilizado junto ao where.
Sintaxe:
```php
WPDatabase::table('table')
            ->where('id = 2')
            ->or()
            ->custom("author = 'Wesley'")
            ->debug();
// SELECT * FROM table1  WHERE id = 2  OR author = 'Wesley' 
```
### Not
O `not()` pode ser utilizado junto ao where.
Sintaxe:
```php
WPDatabase::table('table')
            ->select()
            ->where('id = 2')
            ->and()
            ->not()
            ->custom("author = 'Wesley'")
            ->debug();
// SELECT * FROM table1  WHERE id = 2  AND NOT author = 'Wesley'
```

### isNull
O `isNull()` pode ser utilizado junto ao where e complementado com um custom.
Sintaxe:
```php
WPDatabase::table('table')
            ->select()
            ->where('campo1')
            ->isNull()
            ->debug();
// SELECT * FROM table1 WHERE campo1 IS NULL'
```

### isNotNull
O `isNotNull()` pode ser utilizado junto ao where e complementado com um custom.
Sintaxe:
```php
WPDatabase::table('table')
            ->select()
            ->where('campo1')
            ->isNotNull()
            ->debug();
// SELECT * FROM table1 WHERE campo1 IS NOT NULL'
```

## Create
Sintaxe:
```php
    WPDatabase::table('table')->create()
```
#### Params
O `create()` aceita parâmetros das seguintes formas:
```php
$data = 
[ 
    'name' => 'The book', 
    'author' => 'Wesley Paulo' 
];
WPDatabase::table('table')
            ->create( $data )
            ->debug();
// INSERT INTO table(name,author) VALUES( 'The Book', 'Wesley Paulo' )
```
OU

```php
WPDatabase::table('table')
            ->create([ 
                'name' => 'The book', 
                'author' => 'Wesley Paulo' 
            ])
            ->debug();
// INSERT INTO table(name,author) VALUES( 'The Book', 'Wesley Paulo' )
```

## Update
Sintaxe:
```php
WPDatabase::table('table')->update()
```
#### Params
O `update()` aceita parametros da seguinte forma:
```php
$data = 
[ 
    'name' => 'The book2', 
    'author' => 'Paulo' 
];
WPDatabase::table('table')
            ->update( $data )
            ->debug();
// UPDATE table SET name='The Book2', author='Paulo'
```
OU

```php
WPDatabase::table('table')
            ->update([ 
                'name' => 'The book', 
                'author' => 'Wesley Paulo' 
            ])
            ->debug();
// UPDATE table SET name='The Book2', author='Paulo'
```


## Delete
Sintaxe:
```php
WPDatabase::table('table')->delete()
```
#### Params
O `delete()` aceita parametros da seguinte forma:
```php
WPDatabase::table('table')
            ->delete( $data )
            ->where('id > 1')
            ->debug();
// DELETE FROM table WHERE id > 1
```
## Joins

### Inner Join
#### Params
O `inner_join()` aceita parametros da seguinte forma:
```php
$table = 'table2';
$on = [ 
    ['table2.id', 'table.name'] ];
WPDatabase::table('table')
            ->select()
            ->innerJoin($table, $on)
            ->debug();
/*
SELECT * FROM table INNER JOIN table2 ON table2.id = table.name
*/

OU

$on = [ 
    ['table2.id', '<>','table.name'] ];
WPDatabase::table('table')
            ->select()
            ->innerJoin($table, $on)
            ->debug();
/*
SELECT * FROM table INNER JOIN table2 ON table2.id <> table.name
*/
```


### Left Join
#### Params
O `left_join()` aceita parametros da seguinte forma:
```php
$table = 'table2';
$on = [ 
    ['table2.id', 'table.name']
];
WPDatabase::table('table')
            ->select()
            ->leftJoin($table, $on)
            ->debug();
/*
SELECT * FROM table LEFT JOIN table2 ON table2.id = table.name
*/

OU

$on = [ 
    ['table2.id', '<>','table.name'] ];
WPDatabase::table('table')
            ->select()
            ->leftJoin($table, $on)
            ->debug();
/*
SELECT * FROM table LEFT JOIN table2 ON table2.id <> table.name
*/
```
### Right Join
#### Params
O `rightJoin()` aceita parametros da seguinte forma:
```php
$table = 'table2';
$on = [ 
    ['table2.id', 'table.name']
];
WPDatabase::table('table')
            ->select()
            ->rightJoin($table, $on)
            ->debug();
/*
SELECT * FROM table RIGHT JOIN table2 ON table2.id = table.name
*/

OU
$table = 'table2';
$on = [ 
    ['table2.id', '<>','table.name']
];
WPDatabase::table('table')
            ->select()
            ->rightJoin($table, $on)
            ->debug();
/*
SELECT * FROM table RIGHT JOIN table2 ON table2.id <> table.name
*/
```
### Full Join
#### Params
O `fullJoin()` aceita parametros da seguinte forma:
```php
$table = 'table2';
$on = [ 
    ['table2.id', 'table.name']
];
WPDatabase::table('table')
            ->select()
            ->fullJoin($table, $on)
            ->debug();
/*
SELECT * FROM table FULL JOIN table2 ON table2.id = table.name
*/
```
## Options
Algumas opções estão disponíveis são:
> orderBy($order, array $by) | $by = [column1,column2,..], $order = ASC|DESC
```php
$table = 'table2';
$on = [ 
    ['table2.id', 'table.name']
];
WPDatabase::table('table')
            ->select()
            ->orderBy('asc', 'id')
            ->debug();
/*
SELECT * FROM table ORDER BY id ASC
*/
```
> limit($n) | $n = int 
```php
$table = 'table2';
$on = [ 
    ['table2.id', 'table.name']
];
WPDatabase::table('table')
            ->select()
            ->limit(10)
            ->debug();
/*
SELECT * FROM table LIMIT 10
*/
```
> offset($n) | $n = int
```php
$table = 'table2';
$on = [ 
    ['table2.id', 'table.name']
];
WPDatabase::table('table')
            ->select()
            ->limit(10)
            ->offset(15)
            ->debug();
/*
SELECT * FROM table LIMIT 10 OFFSET 15
*/
```
> like($patternt) | $pattern.. more info [here](https://www.w3schools.com/sql/sql_like.asp)
```php
$table = 'table2';
$on = [ 
    ['table2.id', 'table.name']
];
WPDatabase::table('table')
            ->select()
            ->where('name')
            ->like('W%')
            ->debug();
/*
SELECT * FROM table WHERE name LIKE 'W%'
*/
```
> between($p1, $p2) | between $p1 AND $p2.. more info [here](https://www.w3schools.com/sql/sql_between.asp)
```php
$table = 'table2';
$on = [ 
    ['table2.id', 'table.name']
];
WPDatabase::table('table')
            ->select()
            ->where('name')
            ->between(2,3)
            ->debug();
/*
SELECT * FROM table WHERE id BETWEEN 2 AND 3
*/
```

## Obtendo Resultados

### All
Retorna todos os resultados da consulta em um Array ou em forma de Objeto.

#### Params
Aceita um parâmetro opcional: all($obj = null), caso você sete true, o resultado retornará
Como um objeto. Caso deixe o parametro vazio, o resultado será retornado normalmente como um Array
Sintaxe:
```php
    $resultado = WPDatabase::table('table')
                ->select()
                ->where('id', 1)
                ->all();
```


### Single
Retorna o primeiro resultado da consulta em um Array ou em forma de Objeto.

#### Params
Aceita um parâmetro opcional: all($obj = null), caso você sete true, o resultado retornará
Como um objeto. Caso deixe o parametro vazio, o resultado será retornado normalmente como um Array
Sintaxe:
```php
    $resultado = WPDatabase::table('table')
                ->select()
                ->where('id', 1)
                ->single();
```
### Confirm
Retorna o número de colunas afetadas pela operação, ou 0, caso nenhuma tenha sido afetada.
Sintaxe:
```php
    $data = ['nome' => 'outro'];
    $resultado = WPDatabase::table('table')
                ->update($data)
                ->where('id', 1)
                ->confirm();
```
### LastId
Retorna a chave primária do último registro feito em uma tabela.
Sintaxe:
```php
    $data = ['nome' => 'ninguem', 'idade' => 27]
    $resultado = WPDatabase::table('table')
                ->create()
                ->lastId();
```


## Próximas Atualizações
[Atualizações](falta.md)