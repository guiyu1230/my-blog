<h1 style="text-align: center;">React Router</h1>

## 1.React Router介绍:
<p style="text-indent: 20px">是什么: react-router在history库的基础上，实现了URL与UI的同步，完整的 React 路由解决方案</p>
<p style="text-indent: 20px">作用: 简单的API和强大的功能 例如代码缓冲加载、动态路由匹配、以及建立正确的位置过渡处理</p>

## 2.SPA的原理
<p style="text-indent: 20px">单页应用的原理用两种，一种是通过hash的变化，改变页面，另一种是通过url的变化改变页面而又不用刷新页面。</p>
<p>hash</p>
<p style="text-indent: 20px">window.location.hash='xxx' 改变hash</p>
<p style="text-indent: 20px">window.addEventListener('hashchange',fun) 监听hash的改变</p>
<p>url</p>
<p style="text-indent: 20px">history.pushState(obj,title,'/url') 改变url  pushState()方法绝不会导致hashchange 事件被激活，就算新的URL和旧的只在hash上有区别。</p>
<p style="text-indent: 20px">window.addEventListener('popstate',fun) 当浏览器向前向后时，触发该事件。</p>

## 3. react-router依赖基础 – history(第三方js库)
<table>
    <tr>
        <th>History下的三类API</td>
        <th>兼容不同浏览器和环境</td>
        <th>URL执行</td>
        <th>技术实现</td>
    </tr>
    <tr>
        <td>createHashHistory</td>
        <td>老浏览器的history: 主要通过hash来实现</td>
        <td>location.hash=***<br>location.replace()</td>
        <td>通过hash来存储不同状态下的history信息</td>
    </tr>
    <tr>
        <td>createBrowserHistory</td>
        <td>高版本浏览器: 通过html5里面的history</td>
        <td>pushState、<br>replaceState</td>
        <td>通过html5里面的history</td>
    </tr>
    <tr>
        <td>createMemoryHistory</td>
        <td>在内存中进行历史记录的存储</td>
        <td>在内存中进行历史记录的存储</td>
        <td>在内存中进行历史记录的存储</td>
    </tr>
</table>

## 4.react-router的上层实现过程
<p style="text-indent: 20px">实现URL与UI界面的同步。其中在react-router中，URL对应Location对象，而UI是由react components来决定的，这样就转变成location与components之间的同步问题。</p>
<img src="./img/react-router.png" width="800" height="400" />

## 5. React Router 4.0的更新和前版本对比:
<p style="text-indent: 20px">5.1react-router: 实现了路由的核心功能</p>
<p style="text-indent: 20px">5.2react-router-dom: 基于react-router，加入了在浏览器运行环境下的一些功能，例如：Link组件，会渲染一个a标签，Link组件源码a标签行; BrowserRouter和HashRouter组件，前者使用pushState和popState事件构建路由，后者使用window.location.hash和hashchange事件构建路由。</p>
<p style="text-indent: 20px;font-weight:bold;">5.2.1router 组件（BrowserRouter，HashRouter）
</p>
<p style="text-indent: 20px">Router是一个外层，最后render的是它的子组件，不渲染具体业务组件。
</p>
<p style="text-indent: 20px">分为HashRouter(通过改变hash)、BrowserRouter(通过改变url)、MemoryRouter</p>
<p style="text-indent: 20px">Router负责选取哪种方式作为单页应用的方案hash或browser或其他的，把HashRouter换成BrowserRouter，代码可以继续运行。</p>
<p style="text-indent: 20px">Router的props中有一个history的对象，history是对window.history的封装，history的负责管理与浏览器历史记录的交互和哪种方式的单页应用。history会作为childContext里的一个属性传下去。</p>
<p style="text-indent: 20px;font-weight:bold;">5.2.2route matching 组件（Route，Switch）</p>
<p style="text-indent: 20px">Route </p>
<p style="text-indent: 20px">负责渲染具体的业务组件，负责匹配url和对应的组件</p>
<p style="text-indent: 20px">有三种渲染的组件的方式：component(对应的组件)、render(是一个函数，函数里渲染组件)、children(无论哪种路由都会渲染)</p>
<p style="text-indent: 20px">Switch </p>
<p style="text-indent: 20px">匹配到一个Route子组件就返回不再继续匹配其他组件</p>
<p style="text-indent: 20px;font-weight:bold;">5.2.3navigation 组件（Link）</p>
<p style="text-indent: 20px">跳转路由时的组件，调用history.push把改变url。</p>

## 6. React Router API应用
**Router**
```
//Router是一个外层，最后render的是它的子组件，不渲染具体业务组件
import { BrowserRouter, HashRouter } from 'react-router-dom'
ReactDOM.render((
  <BrowserRouter>
    <App />
  </BrowserRouter>
), document.getElementById('root'))
```
**Switch**
```
//Switch将遍历它的 children 元素（路由），然后只匹配第一个符合的 pathname
//Player组件传递参数可用this.props.match.params.number获取
//exact能够使得路由的匹配更严格。
<Switch>
    <Route exact path='/roster' component={FullRoster}/>
    <Route path='/roster/:number' component={Player}/>
</Switch>
```
**Route**
1. component - 一个 React 组件，当一个带有 component prop 的路由匹配的时候，路由将会返回 prop 提供的 component 类型的组件（通过 React.createElement 渲染）。
2. render - 一个返回 React 元素的方法，与 component 类似，也是当路径匹配的时候会被调用。写成内联形式渲染和传递参数的时候非常方便。
3. children - 一个返回 React 元素的方法。与前两种不同的是，这种方法总是会被渲染，无论路由与当前的路径是否匹配。
```
<Route path='/page' component={Page} />
const extraProps = { color: 'red' }
<Route path='/page' render={(props) => (
  <Page {...props} data={extraProps}/>
)}/>
<Route path='/page' children={(props) => (
  props.match
    ? <Page {...props}/>
    : <EmptyPage {...props}/>
)}/>
```
一般来说，我们一般使用 component 或者 render，children 的使用场景不多，而且一般来说当路由不匹配的时候最好不要渲染任何东西。在我们的例子中，不需要向路由传递任何参数，所以我们一般使用 component。

**Link**
```
import { Link } from 'react-router-dom'
const Header = () => (
  <header>
    <nav>
      <ul>
        <Link to={`/users/${user.id}`}>{user.name}</Link>
        // 修改 activeClassName
        <Link to={`/users/${user.id}`} activeClassName="current">{user.name}</Link>
        // 当链接激活时，修改它的样式
        <Link to="/users" style={{color: 'white'}} activeStyle={{color: 'red'}}>Users</Link>
      </ul>
    </nav>
  </header>
)
```
**路由跳转**
```
// 1. react-router-dom 的Redirect功能, 优雅的实现页面跳转
import { Redirect } from 'react-router-dom';
 
handleOnClick = () => {
  this.setState({redirect: true});
}
 
render() {
  if (this.state.redirect) {
    return <Redirect push to="/sample" />;
  }
  return <button onClick={this.handleOnClick} type="button">Button</button>;
}
//<Redirect to={{
//  pathname: '/login',
//  search: '?utm=your+face',
//  state: { referrer: currentLocation }
//}}/>
```
```
// 2. react-router-dom 的withRouter功能, 优雅的实现页面跳转
import { NavLink, withRouter} from "react-router-dom"
class Nav extends React.Component{
    handleClick = () => {
        // Route 的 三个对象 history, location, match就会被放进这个组件的props属性中.
        console.log(this.props);
        this.props.history.push("/login")
    }
    render() {
        return (
            <div className={'nav'}>
                <span onClick={this.handleClick}>登陆</span>
                <div><NavLink to="/login">登录</NavLink></div>
            </div>
        );
    }
}

// 导出的是 withRouter(Nav) 函数执行
export default withRouter(Nav)
```