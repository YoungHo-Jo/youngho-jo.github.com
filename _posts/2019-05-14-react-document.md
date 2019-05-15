---
published: true
layout: post
date: '2019-05-14 18:06 +0900'
show_meta: true
comments: true
category: react
tags:
  - javascript
  - front-end
toc: true
---
# React Document 읽기!
#React


## Main Concepts

### Hello world!
[Hello World – React](https://reactjs.org/docs/hello-world.html)
가장 작고 기초적인 React Component
```javascript
ReactDOM.create(
	<h1>Hello, world</h1>, // root로 찾은 div안에 넣을 녀석
	document.getElementById('root') // HTML body에 있는 div
);
```


- - - -
### Introducing JSX
[Introducing JSX – React](https://reactjs.org/docs/introducing-jsx.html)

```javascript
const element = (
	<h1 className="greeting">
		Hello, world!
	</h1>
)
```
HTML도 아니고, JS도 아닌 녀석

이는 **Babel**에 의해서 변환된다.
```javascript
// 위의 코드가 변환되면 아래와 같다
const element = React.createElement(
	'h1',
	{ className: 'greeting' },
	'Hello, world!'
);
```
HTML보다 JS에 가깝기 때문에, camelCase로 작성한다.

~Injection Attacks~
에 안전하다. JSX를 렌더링하기 전에 들어있는 값들은 모두 제거한 상태에서 시작하기 때문에, 렌더링 과정에 Injection Attack이 일어날 수 없다. 
모든 것은 렌더링되기전 String으로 변환되기 때문에 XSS에 대해서도 안전하다.

- - - -
### Rendering Elements
[Rendering Elements](https://reactjs.org/docs/rendering-elements.html)

`ReactDOM`을 활용하여 HTML에서 `<div id=“root”></div>`를 찾는다. 찾은 `div`에 우리가 만든 Component들을 집어넣게 되는 것이다. 즉, 기존 App에 React를 통합하려면, 위의 tag를 추가하고 렌더링하면 된다.

```javascript
ReacTDOM.render(
	<h1>hello, world!</h1>,
	document.getElementById('root')
);
```

~React Element~는 **변경불가능한(Immutable)** 녀석이다.
따라서, 내용을 변경하기 위해서는 **새롭게 Element 를 만들어서 교체해준다.**
```javascript
function tick() {
	const element {
		<div>
			<h1>Hello, world!</h1>
			<h2>It is {new Date().toLocaleTimeString()}.</h2>
		</div>
	};

	// 새롭게 만들어 교체해준다.
	ReactDOM.render(element, document.getElementById('root'));
}

// 1초 마다 교체해주게 된다.
setInterval(tick, 1000);
```

하지만! **React는 이전과 현재를 비교하여 필요할 경우에만 업데이트**한다!

- - - -
### Components and Props
[Components and Props](https://reactjs.org/docs/components-and-props.html)

~function component~
```javascript
function Welcome(props) { // Component는 props 파라미터르를 받는다!
	return <h1>Hello, world!</h1>
}
```

~ES6 class~
```javascript
class Welcome extends React.Component {		
	// Component는 항상 대문자로 시작!
	render() {
		return <h1>Hello, world!</h1>
	}
}
```

클래스는 추가적인 기능을 사용할 수 있으며, 간결하게 사용하기 위해서는 function component를 주로 사용

~Props~는 하나의 오브젝트로 전달된다. 

#### Extracting Components
**더 작은 단위로 분리하는 것을 두려워 말자!**

```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
props로 `authro`, `text`, `date`를 받고 있는데, 만약 이들 중 하나라도 변경되면 위의 element가 전체 교체되어야 하므로, **비효율적**이다. 또한, 재사용하기 불가능하다.

아래와 같이 나눈다.
```javascript
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}

function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}

function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
Component의 Props를 네이밍할때는, 어떤 흐름상에 맞추기보다 해당 컴포넌트 입장에서의 명칭을 하는 것이 좋다.

#### Props and Read-Only
**모든 React Components는 props에 따라 생성되는, Pure functions이어야 한다.**


- - - -
### State and LifeCycle
[State and Lifecycle – React](https://reactjs.org/docs/state-and-lifecycle.html)

위의 방법들은 컴포넌트를 제대로 재사용한것이라 할 수 없다. 
리액트에서는 ~local state, life cycle~를 활용해 컴포넌트를 **Encapsulation**하고 제대로 재사용할 수 있게 한다. 

아래는 완벽히 재사용하고 Encapsulized된 component 이다.
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props); // function Clock(props)의 props를 받는것과 동일
    this.state = {date: new Date()};
  }

	// DOM에 추가되면 React는 이 함수를 부른다. 
  componentDidMount() {
		// this에 변수를 추가하여 interval id로 활용
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

	// DOM에서 제거될때 React는 이 함수를 부른다.
  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

1. `<Clock />`이 `ReactDOM.render()`에 전달될떄, `constructor(props)`를 부른다. `this.state`를 현재 시각으로 초기화한다.
2. `render()`를 부른다. React는 어떻게 화면에 출력할지 정보를 얻고, 그에 맞게 DOM을 업데이트한다.
3. `Clock`이 DOM에 추가되면, `componentDidMount()`를 호출한다. `tick()`을 1초에 한번씩 부르도록 브라우저에 요청한다.
4. `tick()`을 부르면 `setState()`로 state를 변경하게 된다. React는 `setState()`를 부르면 state가 변경됨을 알고, `render()`를 부른다. 이를 통해 DOM을 업데이트한다.
5. DOM에서 제거되면, `componentWillUnmount()`를 호출한다. timer가 멈추게 된다.

#### `setState`사용시 유의할 점!

```javascript
/*
	직접적인 변경하지 말 것
*/
// Wrong
this.state.comment = "Hello";
// Correct
this.setState({
	comment: "Hello"
});


/*
	props와 state는 비동기적으로 업데이트된다
*/
// Wrong
this.setState({
	counter: this.state.counter + this.props.increment
});
// Correct
this.setState((state, props) => ({
	counter: state.counter + props.increment
}))

/*
	state는 병합형태로 업데이트된다
*/
constructor(props) {
	super(props);
	this.state = {
		posts: [],
		comments: []
	}
}

componentDidMount() {
	fetchPosts().then(response => {
		this.setState({
			posts: response.posts
		});
	});
	
	// comments만 교체되며, posts는 그대로 남는다!
	// 즉 병합되는 방식으로 작동
	fetchComments().then(response => {
		this.setState({
			comments: response.comments
		});
	});
}
``` 


#### The data flows down!
데이터는 부모에서 자식으로 흐르기만 한다.

~Top-down~
즉, 한 컴포넌트가 가진 데이터는 그 아래의 컴포넌트에게만 영향을 준다.

- - - -
### Handling Events
**default behavior를 방지하기 위해 `false`를 리턴할 수 없다!**
대신 `preventDefault()`를 이용한다.

listeners를 등록하기 위해 `addEventListener`를 이용해 DOM에 등록할 필요가 없다. 단지 전달만 해주면 된다.
```javascript
function ActionLink(e) {
	e.preventDefault(); // e: synthetic event
	console.log('The link was clicked!');

	return (
		<a href="#" onClick={handleClick}>
			Click me
		</a>
	);
}
```
전달되는 `e`는 synthetic event이다. **W3C spec**에 맞게 만들어졌으므로, 브라우저를 가리지 않는다.

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? ‘ON’ : ‘OFF’}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById(‘root’)
);
```
`this.handleClick`은 `bind(this)`를 해주지 않으면 JSX에서 `undefiend`이다. 이는 JS특성으로 자동적으로 class의 함수를 연결해주지않기 때문이다. `onClick={this.handleClick}`처럼, `()`없이 전달하려면 `bind(this)`는 필수다. 

`bind(this)`하지 않고 사용하려면 아래와같이 사용한다.
```javascript
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}

class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```
아래의 `LoginButton`은 `render()`가 불릴때마다 callback을 생성하기 때문에 성능하락이 있다. 

#### Passing arguments to event handlers
```javascript
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
``` 
~arrow function~을 활용하는 것은 e의 위치가 관련은 없다.
하지만, `bind(this)`를 활용하려면, 마지막에 자동으로 붙는다. 

- - - -
### Conditional rendering

[Conditional rendering](https://reactjs.org/docs/conditional-rendering.html)


- - - -
### Lists and Keys
```javascript
function NumberList(props) {
  const numbers = props.numbers;
	// key를 전달하지 않으면 Warning 발생
	// 기본적으로 list의 index로 key가 설정됨
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
배열을 통해 element를 전달하면, key를 전달해야한다. 즉 key는 sibling에선 유일하여 식별가능한 key인 것. 따라서, 전달하지않으면 기본적으로 list의 index를 전달한다. 하지만, 순서의 변경이 잦다면 매우 성능적으로 하락이 발생한다.  
[Index as a key is an anti-pattern – Robin Pokorny – Medium](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)
[in-depth explanation about why keys are necessary](https://reactjs.org/docs/reconciliation.html#recursing-on-children)

#### Extracting components with keys
```javascript
function ListItem(props) {
	/*
	const value = props.value;
	return (
		// Wrong! There is no need to specify the key here
		<li key={value.toString()}>
			<value}
		</li>
	)
	*/

  // Correct! There is no need to specify the key here:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Correct! Key should be specified inside the array.
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```
즉, key는 `map`내부의 element에 대해 필요한 것.
**Sibling**들이 생김

- - - -
### Forms
[Forms](https://reactjs.org/docs/forms.html)
- - - -
### Lifting State Up
[Lifting State Up](https://reactjs.org/docs/lifting-state-up.html)

상태를 상위의 컴포넌트로 올리고자 할때가 종종 있다.
이때는, 상위 컴포넌트로 올리고자하는 값을 state로 관리하게 하고, 해당 컴포넌트로 값 변경 callback함수를 전달한다. 그리고, 해당 컴포넌트는 state를 변경하는것이 아니라, callback함수를 실행하게 하면 상위 컴포넌트의 props가 변경되어 해당 컴포넌트도 변경된다. 


- - - -
### Composition vs Inheritance
**Inehritance보다 Composition을 추천**
Facebook에서는 Inheritnace형식의 Components는 없다.

#### Containment
`props.children`을 활용한다.
```javascript
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}

function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```
`WelcomDialog()`는 `FancyBorder`를 rendering할떄 하위의 children들을 props로 전달한다. `FancyBorder()`에 전달되어 렌더링된다.

드물게 아래와 같은 방식으로도 가능하다.
```javascript
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```


#### Specialization
`WelcomeDialog`는 `Dialog`의 Special case로 생각하여, **generic**하게 만드는 방법이있다.


```javascript
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```

- - - -
### Thinking in React
**Facebook과 Instagram**을 개발하면서 **스케일이 큰 웹앱을 개발하기 좋다**고 느낌

~UI을 개발하는 과정~
1. UI를 작은 Component로 구분하여 Hierarchty를 그린다.
2. Static version을 만든다.
상호작용이 없는 버전. State를 사용하지 않고 개발하는 것!
Top-down과 Bottom-up방식이 있는데, 쉬운방법은 Top-down이고, 큰 규모의 웹앱은 Bottom-up이 좋다.
3. UI State를 표현할 가장 작으면서 완전한 것을 식별한다.
**DRY: Don’t Repeat Yourself**
에로, TODO list를 만들때 TODO 아이템들을 배열로 관리하지만, count를 독립된 state variable로 두지 말라는 것. TODO count를 얻고싶으면 단순히 배열의 길이를 사용하라.
4. 어디서 State를 관리할지 정하자.
	* State에 따라 렌더링되는 모든 컴포넌트를 식별한다.
	* 공통의 소유 컴포넌트를 찾는다.
	* 공통 소유 컴포넌트나 더 높은 컴포넌트가 State를 가져야한다.
	* 찾기 어렵다면, State만 소유하는 컴포넌트를 만들어 상위에 둔다.
5. 역방향 Data flow를 추가한다.

- - - -
## Advanced Guides

### Accessibility
[Accessibility](https://reactjs.org/docs/accessibility.html)

- - - -
### Code-Spliting
[Code-Splitting – React](https://reactjs.org/docs/code-splitting.html)
대부분의 경우 **Webpack**이나 **Browserify**로 bundling한
다.
~Bundling~은 여러 파일을 불러와 하나의 bundle 파일로 만들어 내는 것이다. 
하지만, **프로젝트가 커지면 파일 하나가 너무 커**진다.
따라서, ~Code-Spliting~이 필요한 것이다. Webpack이나 Browserify에 의해 지원된다. 이를 통해 런타임시에 필요에 따라 동적으로 로드되게 한다. 
Code-spliting은 ~Lazy load~를 가능하게 한다. 이는 성능을 향상시켜주며 사용자가 사용하지 않을 코드를 로드하지 않게 하여 효율적이다. 

~방법~
* import()
* React.lazy
* Route based 

#### import()

**Before:**
```javascript
import { add } from './math';
console.log(add(16, 26));
```

**After:**
```javascript
import('./math').then(math => {
	console.log(math.add(16, 26);
});
```

**Webpack**이 이런 문법을 만나면 자동적으로 code-spliting을 시작한다. **Create-React-App**을 사용하면 미리 설정되어있다. **Babel**은 **babel-plugin-syntax-dynamic-import**를 설치해야한다. 

#### React.lazy
React.lazy와 Suspense는 server-side rendering을 지원하지 않는다. 원한다면 **Loadable Components**를 사용한다.
[guild for bundle spliting with server-side rendering](https://github.com/smooth-code/loadable-components/blob/master/packages/server/README.md)

**Before:**
```javascript
import OtherComponent from './OtherComponent';

function MyComponent() {
  return (
    <div>
      <OtherComponent />
    </div>
  );
}
```

**After:**
```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <OtherComponent />
    </div>
  );
}
```
OtherComponent는 rendering될때 load된다.

만약, `MyComponent`를 렌더링하는데, `OtherComponent`가 아직 로드되지 못했다면, 사용자에게 로딩중임을 알리는 것을 표시해줄 필요가 있다. 이떄 **Suspense**를 사용한다.
```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

또한, lazy 컴포넌트를 렌더링하지 못했을떄 에러를 표시할 필요가 있다. **Error Boundaries**를 사용한다.
[Error Boundaries](https://reactjs.org/docs/error-boundaries.html)
```javascript
import MyErrorBoundary from ‘./MyErrorBoundary’;
const OtherComponent = React.lazy(() => import(‘./OtherComponent’));
const AnotherComponent = React.lazy(() => import(‘./AnotherComponent’));

const MyComponent = () => (
  <div>
    <MyErrorBoundary>
      <Suspense fallback={<div>Loading…</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </MyErrorBoundary>
  </div>
);
```

#### Route-based code spliting
개발과정중에 어디에서 code spliting을 할지 애매하다. 그래서 좋은 방법은 Route를 통해 구분하는 것이다. 
`React Router`와 `React.lazy`를 활용해 달성할 수 있다.
```javascript
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import React, { Suspense, lazy } from 'react';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

#### Named Exports
`React.lazy`는 default exports만 지원한다.
```javascript
// MyComponent.js
export { MyComponent as default } from "./ManyComponents.js";
```
로 reexport하면 된다.

- - - -
### Context
[Context – React](https://reactjs.org/docs/context.html)
~Context~는 수동으로 props를 일일이 전달하지 않고, Component tree를 따라서 데이터를 전달하는 방법을 제공한다.
매우 자주 사용되는 것들을 내려보내는데 유용하다.

#### When to use
데이터가 React Components들에게 있어 **글로벌**할때 사용하도록 만들어졌다. 
(현재 인증된 사용자 정보, 테마, 선호되는 언어 등)

```javascript
class App extends React.Component {
	// 가장 상위 component로, dark theme을 props로 전달한다. 
  render() {
    return <Toolbar theme="dark" />;
  }
}

function Toolbar(props) {
  // The Toolbar component must take an extra "theme" prop
  // and pass it to the ThemedButton. This can become painful
  // if every single button in the app needs to know the theme
  // because it would have to be passed through all components.
	// 상위로 붙어 받은 theme을 또다시 아래로 전달
  return (
    <div>
      <ThemedButton theme={props.theme} />
    </div>
  );
}

class ThemedButton extends React.Component {
	// 전달받은 theme을 다시 또 전달
  render() {
    return <Button theme={this.props.theme} />;
  }
}
```
를 아래와 같이 변경하여 사용할 수 있다.
```javascript
// Context lets us pass a value deep into the component tree
// without explicitly threading it through every component.
// Create a context for the current theme (with "light" as the default).
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Use a Provider to pass the current theme to the tree below.
    // Any component can read it, no matter how deep it is.
    // In this example, we're passing "dark" as the current value.
		// Context를 통해 통합 전달을 구현
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// A component in the middle doesn't have to
// pass the theme down explicitly anymore.
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // Assign a contextType to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```


#### Before you use context
~Context~는 여러 계층의 많은 Component들이 공통적으로 사용하고자할때 사용한다. 하지만 너무 많이 사용할 경우 Component 재사용성을 해친다. 
단순히 props를 여러레벨 거쳐 전달하는것이 귀찮다면 ~component composition~을 활용해야 한다. 

```javascript
<Page user={user} avatarSize={avatarSize} />
// ... which renders ...
<PageLayout user={user} avatarSize={avatarSize} />
// ... which renders ...
<NavigationBar user={user} avatarSize={avatarSize} />
// ... which renders ...
<Link href={user.permalink}>
  <Avatar user={user} size={avatarSize} />
</Link>
```
같은 데이터를 사용하는 여러 컴포넌틀에게 동일하게 전달하는 일이 매우 불필요해보이긴한다. 또, 하위 컴포넌트에서 새로운 props를 전달해야한다면 최초 전달하는 상위 컴포넌트까지 props를 받을 수 있도록 작업을 해야한다.

context 없이 위와 같은 어려움을 해결하기 위해서는 props로 component를 전달하는 것이다.
```javascript
function Page(props) {
  const user = props.user;
  const userLink = (
    <Link href={user.permalink}>
      <Avatar user={user} size={props.avatarSize} />
    </Link>
  );
  return <PageLayout userLink={userLink} />;
}

// Now, we have:
<Page user={user} avatarSize={avatarSize} />
// ... which renders ...
<PageLayout userLink={...} />
// ... which renders ...
<NavigationBar userLink={...} />
// ... which renders ...
{props.userLink}
```
~Inversion of Control~이라고 불리는 이 방법은 코드를 깔끔하게 하지만 상위 컴포넌트를 복잡하게 만든다. 따라서 모든 경우에 대해 올바른 방법은 아니며 하위 컴포넌트를 더욱더 유연하게 만들지않으면 구현하기 어렵다. 


~React.createContext~
```javascript
const MyContext = React.createContext(defaultValue);
```
이 context를 sub하는 컴포넌트가 렌더링될때 값을 읽어서 상위 컴포넌트에서 가장 가깝게 일치하는 `Provider`를 찾는다.

~Context.Provider~
```javascript
<MyContext.Provider value={/* some value */}>
```
`Provider`에 감싸진 컴포넌트들은 Context가 변화하는 것을 sub한다. 하나의 `Provider`에는 여러 consumer(일반적인 component)가 연결될 수 있다. 또한, 여러개 겹쳐서 사용할 수 있다.
모든 consumer들은 `Provider`가 제공하는 값이 변경되면 재렌더링한다. 


~Class.contextType~
```javascript
class MyClass extends React.Component {
  componentDidMount() {
    let value = this.context;
    /* perform a side-effect at mount using the value of MyContext */
  }
  componentDidUpdate() {
    let value = this.context;
    /* ... */
  }
  componentWillUnmount() {
    let value = this.context;
    /* ... */
  }
  render() {
    let value = this.context;
    /* render something based on the value of MyContext */
  }
}
MyClass.contextType = MyContext;
``` 
`contextType`는 `React.createContext()`로 만들어진 Context object로 할당된다. `this.context`를 통해 접근이 가능하며 모든 lifeCycle에서 접근이가능하다. 
하나의 context만 sub가능하다. [Consuming Mutiple Contexts](https://reactjs.org/docs/context.html#consuming-multiple-contexts)
**public class fields syntax**를 사용하면 아래와같이 사용해야한다.
[JavaScript Private and Public Class Fields](https://tylermcginnis.com/javascript-private-and-public-class-fields/)
```javascript
class MyClass extends React.Component {
  static contextType = MyContext;
  render() {
    let value = this.context;
    /* render something based on the value */
  }
}
```

~Context.Consumer~
```javascript
<MyContext.Consumer>
  {value => /* render something based on the context value */}
</MyContext.Consumer>
```
context 변화를 sub하는 component이다. function component안에서 sub할 수 있게 해준다. 

#### Examples
~Dynamic Context~
```javascript
/* theme-context.js */
export const themes = {
  light: {
    foreground: '#000000',
    background: '#eeeeee',
  },
  dark: {
    foreground: '#ffffff',
    background: '#222222',
  },
};

export const ThemeContext = React.createContext(
  themes.dark // default value
);
/* END OF theme-context.js */

/* theme-button.js */
import {ThemeContext} from './theme-context';

class ThemedButton extends React.Component {
  render() {
    let props = this.props;
    let theme = this.context;
    return (
      <button
        {...props}
        style={{backgroundColor: theme.background}}
      />
    );
  }
}
ThemedButton.contextType = ThemeContext;

export default ThemedButton;
/* END OF theme-button.js */

/* app.js */
import {ThemeContext, themes} from './theme-context';
import ThemedButton from './themed-button';

// An intermediate component that uses the ThemedButton
function Toolbar(props) {
  return (
    <ThemedButton onClick={props.changeTheme}>
      Change Theme
    </ThemedButton>
  );
}

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      theme: themes.light,
    };

    this.toggleTheme = () => {
      this.setState(state => ({
        theme:
          state.theme === themes.dark
            ? themes.light
            : themes.dark,
      }));
    };
  }

  render() {
    // The ThemedButton button inside the ThemeProvider
    // uses the theme from state while the one outside uses
    // the default dark theme
    return (
      <Page>
        <ThemeContext.Provider value={this.state.theme}>
          <Toolbar changeTheme={this.toggleTheme} />
        </ThemeContext.Provider>
        <Section>
          <ThemedButton />
        </Section>
      </Page>
    );
  }
}

ReactDOM.render(<App />, document.root);
/* END OF app.js */
```

~Updating context from a nested component~
```javascript
/* theme-context.js */
// Make sure the shape of the default value passed to
// createContext matches the shape that the consumers expect!
export const ThemeContext = React.createContext({
  theme: themes.dark,
  toggleTheme: () => {}, // nested component위해 함수로 만들어 전달 
});
/* END OF theme-context.js */

/* theme-toggler-button.js */
import {ThemeContext} from './theme-context';

function ThemeTogglerButton() {
  // The Theme Toggler Button receives not only the theme
  // but also a toggleTheme function from the context
  return (
    <ThemeContext.Consumer>
      {({theme, toggleTheme}) => (
        <button
          onClick={toggleTheme}
          style={{backgroundColor: theme.background}}>
          Toggle Theme
        </button>
      )}
    </ThemeContext.Consumer>
  );
}

export default ThemeTogglerButton;
/* END OF theme-toggler-button.js */

/* app.js */
import {ThemeContext, themes} from './theme-context';
import ThemeTogglerButton from './theme-toggler-button';

class App extends React.Component {
  constructor(props) {
    super(props);

    this.toggleTheme = () => {
      this.setState(state => ({
        theme:
          state.theme === themes.dark
            ? themes.light
            : themes.dark,
      }));
    };

    // State also contains the updater function so it will
    // be passed down into the context provider
    this.state = {
      theme: themes.light,
      toggleTheme: this.toggleTheme,
    };
  }

  render() {
    // The entire state is passed to the provider
    return (
      <ThemeContext.Provider value={this.state}>
        <Content />
      </ThemeContext.Provider>
    );
  }
}

function Content() {
  return (
    <div>
      <ThemeTogglerButton />
    </div>
  );
}

ReactDOM.render(<App />, document.root);
/* END OF app.js */

```


~Consuming multiple context~
re-rendering을 빠르게 하기 위해 context를 구분하여 사용하게되어 아래와 같이 여러개의 context를 구독하면된다.
```javascript
// Theme context, default to light theme
const ThemeContext = React.createContext('light');

// Signed-in user context
const UserContext = React.createContext({
  name: 'Guest',
});

class App extends React.Component {
  render() {
    const {signedInUser, theme} = this.props;

    // App component that provides initial context values
    return (
      <ThemeContext.Provider value={theme}>
        <UserContext.Provider value={signedInUser}>
          <Layout />
        </UserContext.Provider>
      </ThemeContext.Provider>
    );
  }
}

function Layout() {
  return (
    <div>
      <Sidebar />
      <Content />
    </div>
  );
}

// A component may consume multiple contexts
function Content() {
  return (
    <ThemeContext.Consumer>
      {theme => (
        <UserContext.Consumer>
          {user => (
            <ProfilePage user={user} theme={theme} />
          )}
        </UserContext.Consumer>
      )}
    </ThemeContext.Consumer>
  );
}
```
자주 묶여서 사용하는 Context가 있다면 이 들을 함친 render용 component르 만들어 재사용하자

전달되는 값에 따라서 하위의 컴포넌트들이 re-render되기 떄문에 의도하지않은 re-render가 발생할 수 있다.
예시:
```javascript
class App extends React.Component {
  render() {
    return (
      <Provider value={{something: 'something'}}>
        <Toolbar />
      </Provider>
    );
  }
}
```
render가 불릴때마다 provider에 의해 전달되는 value가 계속해서 새로운 오브젝트로 할당되기 때문에 하위 컴포넌트들은 항상 re-render하게 된다. 

해결방법:
```javascript
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: {something: 'something'},
    };
  }

  render() {
    return (
      <Provider value={this.state.value}>
        <Toolbar />
      </Provider>
    );
  }
}
```

- - - -
### Error Boundaries
version 16에서 추가된 기능
부분적인 에러가 웹앱 전체에 미치는 영향을 최소화화기 위해 만들었다.
> catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI  
에러를 잡아내고 기록하며 실패시에 사용자에게 유용한 정보를 보여주는 일련의 프로세스를 거친다.

하지만 아래의 경우에는 사용할 수 없다.
* Event handlers
* Aynchronous code(setTimeout, requestAnimationFrame)
* Server side rendering
* Errors thrown in the error boundary itself(rather than its children)

`static getDerivedStateFromError()`나 `componentDidCatch()`를 통해 구현한다.
전자는 fallback UI를 보여주기위해 사용하고, 후자는 logging하기 위해 사용한다.
```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info);
  }

  render() {
		// fallback으로 에러를 사용자에게 표시
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}

// 사용
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

적절한 위치에 두고 사용하면 된다.

#### New behavior for Uncaught Errors
16버전에서 Error boundary로 처리되지 않은 에러들은 전체 React component tree를 unmount하게 된다.

Facebook의 Messenger는 sidebar, info panel, conversion log, message input을 각각 error boundary로 감싸서 이 들중 하나가 충돌하면 나머지가 작동하지 않도록 개발하였다.

또한, **JS error reporting service**를 사용하길 권장한다.

#### Component Stack Traces
