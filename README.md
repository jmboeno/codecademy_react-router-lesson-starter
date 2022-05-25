# Learn React Router
### O que é React Router?
---
O React Router é uma biblioteca que fornece componentes de navegação para desenvolvedores do React para criar aplicativos de página única (SPAs) com roteamento dinâmico do lado do cliente.
Os aplicativos que usam o React-Router podem se beneficiar da separação de conteúdo oferecida aos aplicativos de várias páginas sem a interrupção na experiência do usuário causada por recarregamentos de página.

### Importando Browser Router
---
Para usar o React Router, o BrowserRoutercomponente (geralmente alias como Router) deve ser importado para o arquivo de componente de nível superior.
Agrupar o componente de nível superior com BrowserRouterdá acesso à árvore de componentes inteiras do seu aplicativo ao React Router.
```javascript
import React from "react";
import { BrowserRouter as Router } from "react-router-dom";
 
export default function TopLevelComponent () {
  return (
    <Router>
      application contents here      
    </Router>
  )
}
```
### Route
---
O componente do React Router  `<Route>` é projetado para renderizar seus filhos quando seu prop  `path` corresponde à URL atual.
O  componente  `<Route>` tem uma prop  booleana `exact` que, quando true, fará com que  `<Route>` renderize seus filhos somente quando a URL atual corresponder exatamente à do  path do componente  `<Route>`. Quando `exact` for false (seu valor padrão), a `<Route>` será renderizado se seu caminho corresponder parcialmente ao URL atual.
```javascript
import React from "react";
import { BrowserRouter as Router, Route } from "react-router-dom";
import Users from "../features/users/Users"
import NewUser from "../features/users/NewUser";
 
export default const App () {
  return (
    <Router>
      <Route path="/users" exact>
        <Users />
      </Route>
      <Route path="/users/new">
        <NewUser />
      </Route>
    </Router>
  )
}
```
###Link
---
O componente  `<Link>` do React Router  pode ser usado para criar links para navegação. O prop `to` especifica o local para o qual o usuário será redirecionado após clicar no arquivo `<Link>`.

A renderização de a `<Link>` irá inserir uma marca de âncora ( `<a>`) em seu documento HTML, mas o comportamento padrão da âncora (acionar o recarregamento de uma página) será desabilitado. Isso permite que os aplicativos `<Router>` respondam às alterações de URL renderizando o conteúdo apropriado.
```javascript
<Link to="/about">About</Link>
```

### NavLink
---
O  componente `<NavLink>` do React Router é um tipo especial de `<Link>` que pode ser estilizado de forma diferente quando o prop `to` do componente corresponde à localização atual.

A  prop `activeClassName` (cujo valor padrão é 'active') especifica a classe que será aplicada quando a prop `to` no `<NavLink>` corresponder à localização atual.

```javascript
<NavLink 
  to="/about" 
  activeClassName="highlighted"
>
  About
</NavLink>
```

### URL Parameters

Parâmetros de URL são segmentos dinâmicos (ou seja, não constantes) da prop `path` de um componente `<Route>`.
Eles podem ser usados para servir recursos dinamicamente com base na localização atual da janela.
Um parâmetro de URL começa com dois pontos e é seguido pelo nome do parâmetro, assim: `:parameter`. 
Para especificar que um parâmetro de URL é opcional, anexe um ponto de interrogação, assim:  `:parameter?`.

import { BrowserRouter as Router, Route } from "react-router-dom"
import Book from "../features/books/Book"

```javascript
function App () {
  return (
    <Router>
      <Route path="/books/:bookId/:page?">
        <Book />
      </Route>
    </Router>
  )
}
```
### useParams()
---
O hook  `useParams()` do React Router pode ser usado por um componente renderizado por a `<Route>` com um caminho dinâmico para obter os nomes e valores dos parâmetros da URL atual.
Esta função retorna um objeto contendo um par chave/valor para cada parâmetro de URL onde a chave é o nome do parâmetro de URL e o valor é o valor atual do parâmetro.
```javascript
import React from "react";
import { useParams } from "react-router-dom";
 
// assume que este componente é renderizado por um <Route> com o caminho "/users/:userName"
export default const UserProfile () {
  const { userName } = useParams()
  return (
    <h1> Welcome {userName}! </h1>
  )
/*
  Se o usuário visitar /users/Codey, o seguinte será renderizado:
  
  <h1> Bem-vindo Codey!
  */
}
```
### Switch
---
O componente `<Switch>` do React Router renderiza o primeiro de seus filhos `<Route>` ou  `<Redirect>` componentes cuja prop `path` corresponde à URL atual.
Ao agrupar vários  componentes  `<Route>` em um `<Switch>`, é importante ordenar os  componentes `<Route>` do mais específico para o menos específico.
```javascript
// Direita: navegar até "/songs/123" fará com que a primeira rota seja renderizada, enquanto navegar até "/songs" fará com que a segunda seja renderizada
<Switch>
  <Route path="/songs/:songId">
    <Song />
  </Route> 
  <Route path="/songs">
    <AllSongs />
  </Route>  
</Switch>
 
// Errado: navegar para "/songs/123" OU "/songs" fará com que a primeira rota seja renderizada. A segunda rota nunca será renderizada.
<Switch>
  <Route path="/songs">
    <AllSongs />
  </Route>  
  <Route path="/songs/:songId">
    <Song />
  </Route> 
</Switch>
```
###useRouteMatch()
---
`<Routes>` pode ser renderizado em qualquer componente que desça do seu arquivo Router. Assim, mesmo os componentes renderizados por um `<Route>` podem renderizar outros  componentes  `<Route>`.

O hook `useRouteMatch()` do React Router ajuda a construir relativas  props   `path` e `to` para os  componentes  `<Route>` e  `<Link>` retornando um  objeto `match` com as  propriedades `url` e  `path`:

* A propriedade `path` é usada para construir a prop ` paths` de um componente `<Route>` aninhado em relação ao pai `<Route>`.

* A propriedade `url` é usada para construir a prop  `to` de um componente  `<Link>` aninhado em relação ao pai `<Route>`.

```javascript
// App.js
import React from "react";
import { BrowserRouter as Router, Route } from "react-router-dom";
import UserProfile from "../features/users/UserProfile";
 
export default function App () {
  return (
    <Router>
      <Route path="/users/:userId">
        <UserProfile />
      </Route>
    </Router>
  )
}
// UserProfile.js
import React from "react";
import { Route, Link, useRouteMatch } from "react-router-dom";
import FriendList from "./FriendList";
 
export default function UserProfile () {
  const { path, url } = useRouteMatch();
  
  return (
    <div>
      <SomeUserProfileInformation/>
	  
      {/* Redireciona para '/users/123/friends' */}
      <Link to={`${url}/friends`}>Friends</Link>
      
      {/* Renderiza <FriendList/> para o caminho '/users/:userId/friends' */}
      <Route path={`${path}/friends`}>
        <FriendList/>
      </Route>
    </div>
  )
}
```
### useHistory()
---
O hook `useHistory()` do React Router retorna uma instância do  objeto history, que possui uma estrutura mutável semelhante a uma pilha que acompanha o histórico da sessão do usuário e contém os seguintes métodos úteis:
`history.push(location)` - redireciona imperativamente o usuário para o local especificado
`go(n)` - Move o ponteiro na pilha de histórico por n entradas
`goBack()` - Equivalente a ir (-1)
`goForward()` - Equivalente a ir(1)

```javascript
import React from "react";
import { useHistory } from "react-router-dom";
 
export default function Footer () {
  const history = useHistory();
  
  return (
    <footer>
      <button onClick={() => history.goBack()}>
        Back
      </button>
      <button onClick={() => history.goForward()}>
        Forward
      </button>
      <button onClick={() => history.push('/about')}>
        About
      </button>
    </footer>
  )
}
```
### Query Parameters
---
Os parâmetros de consulta aparecem em URLs que começam com um ponto de interrogação ( ?) e são seguidos por um nome de parâmetro atribuído a um valor. Eles são opcionais e são usados com mais frequência para pesquisar, classificar e/ou filtrar recursos.
Por exemplo, se você visitar o URL fornecido, será direcionado para a `/search` página do Google exibindo resultados para o termo de pesquisa `'codecademy'`. Neste exemplo, o nome do parâmetro de consulta é q.
```
https://www.google.com/search?q=codecademy
```
### useLocation()
---
O hook `useLocation()` do React Router retorna um objeto cuja propriedade  `search` corresponde à string de consulta da URL atual.
Passar a string de pesquisa para o construtor  `URLSearchParams` produz um objeto cujo método `.get(paramName)` retorna o valor de paramName.
```javascript
// Se o usuário visitar /search/?term=codecademy...
const { search } = useLocation();
// O valor da pesquisa seria '?term=codecademy'
const queryParams = new URLSearchParams(search);
// queryParams é um objeto com um método .get()...
const termValue = queryParams.get('term');
// ... e termValue seria 'codecademy'
```

###End
