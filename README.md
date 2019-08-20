================================== REACT ROUTER ==================================

----------------------- Basic -----------------------

  import { default as Router, Route } from 'react-router'

  const routes = (
    <Route>
      <Route path='*' handler={RootView} />
    </Route>
  )

  Router.run(routes, Router.HashLocation, (Root) => {
    React.render(<Root />, document.getElementById('all'))
  })


----------------------- Nesting -----------------------

  const routes = (
    <Route handler={Chrome}>
      <Route path='about' handler={About} />
      <Route path='inbox' handler={Inbox} />
      <Route path='messages/:id' handler={Message} />
    </Route>
  )

  import { RouteHandler } from 'react-router'

  const Chrome = React.createClass({
    render () {
      return (
        <div>
          <h1>App</h1>
          <RouteHandler />
        </div>
      )
    }
  })


----------------------- URL params -----------------------

  var Message = React.createClass({
    componentDidMount: function () {
      // from the path `/inbox/messages/:id`
      var id = this.props.params.id
      ...


----------------------- Link -----------------------

  import { Link } from 'react-router'

  <!-- make a named route `user` -->
  <Link to='user' params={{userId: 10}} />

  <Link to='login'
    activeClassName='-active'
    onClick='...'>


----------------------- Other config -----------------------

  <Route path='/'>
    <DefaultRoute handler={Home} />
    <NotFoundRoute handler={NotFound} />
    
    <Redirect from='login' to='sessions/new' />
    <Redirect from='login' to='sessions/new' params={{from: 'home'}} />
    <Redirect from='profile/:id' to='about-user' />

    <Route name='about-user' ... />


----------------------- Router.create -----------------------

  var router = Router.create({
    routes: <Route>...</Route>,
    location: Router.HistoryLocation
  })

  router.run((Root) => { ... })


----------------------- Navigation -----------------------

import { Navigation } from 'react-router'

  React.createClass({
    mixins: [ Navigation ], ...
  })

  this
    .transitionTo('user', {id: 10})
    .transitionTo('/path')
    .transitionTo('http://...')
    .replaceWith('about')
    .makePath('about') // return URL
    .makeHref('about') // return URL
    .goBack()


  ================================== LAZY LOADING ==================================

  ![](https://raw.githubusercontent.com/asifvora/lazy-loading-react-js/master/lazy2.gif)

  For loading data from a database into memory it's handy to design things so that as you load an object of interest you also load the objects that are related to it. This makes loading easier on the developer using the object, who otherwise has to load all the objects he needs explicitly.

  However, if you take this to its logical conclusion, you reach the point where loading one object can have the effect of loading a huge number of related objects - something that hurts performance when only a few of the objects are actually needed.

  A Lazy Load interrupts this loading process for the moment, leaving a marker in the object structure so that if the data is needed it can be loaded only when it is used. As many people know, if you're lazy about doing things you'll win when it turns out you don't need to do them at all.
