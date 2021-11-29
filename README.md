# hooks_text
```
import React, { useState } from 'react';

function Example() {
  // Объявление переменной состояния, которую мы назовём "count"  
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Вы кликнули {count} раз</p>
      <button onClick={() => setCount(count + 1)}>
        Нажми на меня
      </button>
    </div>
  );
}
```

###useContext
```
const value = useContext(MyContext);
```
Пример без контекста
```
class App extends React.Component {
  render() {
    return <Toolbar theme="dark" />;
  }
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton theme={props.theme} />
    </div>
  );
}

class ThemedButton extends React.Component {
  render() {
    return <Button theme={this.props.theme} />;
  }
}
```

Пример с контекстом:
```
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// Компонент, который находится в середине,
// больше не должен явно передавать тему вниз.
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

Пример с useContext:
```
const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);  
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      Я стилизован темой из контекста!
    </button>
  );
}
```

###useMemo
```
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
Пример:
```
function Parent({ a, b }) {
  // Рендерится повторно, только если `a` изменится:
  const child1 = useMemo(() => <Child1 a={a} />, [a]);
  // Рендерится повторно, только если `b` изменится:
  const child2 = useMemo(() => <Child2 b={b} />, [b]);
  return (
    <>
      {child1}
      {child2}
    </>
  )
}
```

###Suspense
```
const ProfilePage = React.lazy(() => import('./ProfilePage'));// Ленивая загрузка

// Показать спиннер, во время загрузки профиля
<Suspense fallback={<Spinner />}>
  <ProfilePage />
</Suspense>
```

**Чем задержка не является**
- Это не реализация получения данных.
- Это не готовый к использованию клиент.
- Задержка не привязывает получение данных к слою представления.

**Что позволяет делать задержка**
- Она позволит глубже интегрировать React в библиотеки получения данных.
- Она позволит вам управлять намеренно спроектированными состояниями загрузки.
- Она позволит избежать состояния гонки.
