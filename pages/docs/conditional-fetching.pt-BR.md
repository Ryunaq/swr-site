# Data Fetching Condicional

## Conditional

Use `null` ou passe a função como `key` para obter dados condicionais. Se a função lança ou retorna um valor `falsy` (um valor que se traduz em falso quando avaliado em um contexto Boolean), SWR não iniciará a requisição.

```js
// fetch condicional
const { data } = useSWR(shouldFetch ? '/api/data' : null, fetcher)

// ...or retornar um valor falsy
const { data } = useSWR(() => shouldFetch ? '/api/data' : null, fetcher)

// ...or lançar um erro quando user.id não está definido
const { data } = useSWR(() => '/api/data?uid=' + user.id, fetcher)
```

## Dependente

SWR também permite você obter dados que dependem de outros dados. Ele garante o máximo possível de paralelismo (evitando o waterfall), assim como a sequência de requisições quando um pedaço de dados dinâmico é necessário para que a próxima requisição de dados seja feita.

```js
function MyProjects () {
  const { data: user } = useSWR('/api/user')
  const { data: projects } = useSWR(() => '/api/projects?uid=' + user.id)

  // Quando passamos uma função, SWR usará o valor de retorno
  // como `key`. Se a função lança ou retorna um valor falsy,
  // SWR saberá que algumas dependências não estão prontas.
  // No caso, `user.id` lança quando `user` não está carregado.

  if (!projects) return 'loading...'
  return 'You have ' + projects.length + ' projects'
}
```
