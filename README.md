# 🎯 Подготовка к собеседованию - Вопросы и Ответы

## 📚 JavaScript

### 1. Что такое замыкание (Closure)?
**Ответ:**
Замыкание - это функция, которая имеет доступ к переменным из внешней области видимости даже после того, как внешняя функция завершила выполнение.

```javascript
function outer() {
  let count = 0;
  return function inner() {
    count++;
    return count;
  };
}
const counter = outer();
console.log(counter()); 
console.log(counter()); 
```

**Применение:** Модули, приватные переменные, мемоизация.

---

### 2. Разница между `var`, `let` и `const`?
**Ответ:**
- **var**: function-scoped, hoisting, можно переопределять
- **let**: block-scoped, hoisting без инициализации (TDZ), можно переназначать
- **const**: block-scoped, нельзя переназначать, но объект можно мутировать

```javascript
if (true) {
  var a = 1;
  let b = 2;
  const c = 3;
}
console.log(a); // 1
console.log(b); // ReferenceError
console.log(c); // ReferenceError
```

---

### 3. Что такое Event Loop?
**Ответ:**
Механизм, который управляет выполнением кода, обработкой событий и выполнением подзадач. Состоит из:
- Call Stack (стек вызовов)
- Web APIs (setTimeout, DOM, fetch)
- Callback Queue (очередь колбэков)
- Microtask Queue (Promise.then, queueMicrotask)

**Приоритет:** Microtasks выполняются перед Callback Queue.

```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
console.log('4');
// Вывод: 1, 4, 3, 2
```

---

### 4. Что такое Promise и async/await?
**Ответ:**
**Promise** - объект, представляющий результат асинхронной операции (pending, fulfilled, rejected).

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve('Success'), 1000);
});

promise
  .then(result => console.log(result))
  .catch(error => console.error(error));
```

**async/await** - синтаксический сахар над Promise:

```javascript
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error(error);
  }
}
```

---

### 5. Разница между `==` и `===`?
**Ответ:**
- `==` - нестрогое сравнение (с приведением типов)
- `===` - строгое сравнение (без приведения типов)

```javascript
'5' == 5;  // true
'5' === 5; // false
null == undefined;  // true
null === undefined; // false
```

---

### 6. Что такое this в JavaScript?
**Ответ:**
`this` - контекст выполнения функции. Зависит от способа вызова:

```javascript
// 1. Глобальный контекст
console.log(this); // window (в браузере)

// 2. В методе объекта
const obj = {
  name: 'John',
  greet() {
    return this.name; // 'John'
  }
};

// 3. В стрелочной функции (лексический this)
const obj2 = {
  name: 'John',
  greet: () => {
    return this.name; // undefined (this из внешней области)
  }
};

// 4. С bind/call/apply
function greet() {
  return this.name;
}
greet.call({ name: 'John' }); // 'John'
```

---

### 7. Что такое деструктуризация?
**Ответ:**
Синтаксис для извлечения значений из массивов или объектов:

```javascript
// Массивы
const [a, b, ...rest] = [1, 2, 3, 4];
// a = 1, b = 2, rest = [3, 4]

// Объекты
const { name, age } = { name: 'John', age: 30 };
// name = 'John', age = 30

// С переименованием
const { name: userName } = { name: 'John' };
// userName = 'John'

// Значения по умолчанию
const { name = 'Anonymous' } = {};
```

---

### 8. Методы массивов: map, filter, reduce
**Ответ:**

```javascript
// map - преобразует каждый элемент
[1, 2, 3].map(x => x * 2); // [2, 4, 6]

// filter - фильтрует элементы
[1, 2, 3, 4].filter(x => x > 2); // [3, 4]

// reduce - сводит к одному значению
[1, 2, 3].reduce((acc, x) => acc + x, 0); // 6

// find - находит первый элемент
[1, 2, 3].find(x => x > 1); // 2

// some/every - проверка условий
[1, 2, 3].some(x => x > 2); // true
[1, 2, 3].every(x => x > 0); // true
```

---

## 📘 TypeScript

### 1. Зачем нужен TypeScript?
**Ответ:**
- Статическая типизация - ошибки на этапе разработки
- Улучшенный автокомплит и рефакторинг
- Документация кода через типы
- Лучшая поддержка больших проектов

---

### 2. Разница между `interface` и `type`?
**Ответ:**

```typescript
// interface - можно расширять и объединять
interface User {
  name: string;
}

interface Admin extends User {
  role: string;
}

// type - более гибкий, union types, computed properties
type Status = 'loading' | 'success' | 'error';

type User = {
  name: string;
};

// Разница:
// - interface можно объявлять несколько раз (merging)
// - type может использовать union, intersection, mapped types
```

---

### 3. Что такое Generics?
**Ответ:**
Обобщенные типы, позволяющие создавать переиспользуемые компоненты:

```typescript
function identity<T>(arg: T): T {
  return arg;
}

identity<string>('hello');
identity<number>(42);

// В React компонентах
interface Props<T> {
  data: T;
  render: (item: T) => React.ReactNode;
}
```

---

### 4. Utility Types: Partial, Pick, Omit
**Ответ:**

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}

// Partial - все поля опциональны
type PartialUser = Partial<User>;
// { id?: number; name?: string; ... }

// Pick - выбрать поля
type UserName = Pick<User, 'name' | 'email'>;
// { name: string; email: string; }

// Omit - исключить поля
type UserWithoutId = Omit<User, 'id'>;
// { name: string; email: string; age: number; }

// Record - создать объект с ключами
type UserMap = Record<string, User>;
```

---

### 5. Что такое Type Guards?
**Ответ:**
Функции для сужения типов:

```typescript
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

function process(value: string | number) {
  if (isString(value)) {
    // TypeScript знает, что value - string
    value.toUpperCase();
  }
}
```

---

## ⚛️ React

### 1. Что такое Virtual DOM?
**Ответ:**
Легковесная копия реального DOM в памяти. React сравнивает Virtual DOM с предыдущей версией (diffing) и обновляет только измененные части реального DOM (reconciliation).

**Преимущества:**
- Быстрее, чем прямая работа с DOM
- Батчинг обновлений
- Кроссплатформенность (React Native)

---

### 2. Жизненный цикл компонента
**Ответ:**

**Классовые компоненты:**
- `componentDidMount` - после монтирования
- `componentDidUpdate` - после обновления
- `componentWillUnmount` - перед размонтированием

**Функциональные компоненты (хуки):**
```javascript
useEffect(() => {
  // componentDidMount + componentDidUpdate
  return () => {
    // componentWillUnmount
  };
}, [dependencies]);
```

---

### 3. Разница между useState и useRef?
**Ответ:**

```javascript
// useState - вызывает ререндер при изменении
const [count, setCount] = useState(0);
setCount(1); // компонент перерендерится

// useRef - не вызывает ререндер
const countRef = useRef(0);
countRef.current = 1; // ререндера не будет

// useRef также для доступа к DOM
const inputRef = useRef(null);
<input ref={inputRef} />
```

---

### 4. Что такое useMemo и useCallback?
**Ответ:**

```javascript
// useMemo - мемоизация значения
const expensiveValue = useMemo(() => {
  return computeExpensiveValue(a, b);
}, [a, b]);

// useCallback - мемоизация функции
const handleClick = useCallback(() => {
  doSomething(a, b);
}, [a, b]);

// Зачем? Предотвращение ненужных ререндеров дочерних компонентов
```

---

### 5. Что такое React.memo?
**Ответ:**
HOC для мемоизации компонента. Компонент ререндерится только если изменились props:

```javascript
const MyComponent = React.memo(({ name, age }) => {
  return <div>{name} - {age}</div>;
}, (prevProps, nextProps) => {
  // кастомная функция сравнения (опционально)
  return prevProps.name === nextProps.name;
});
```

---

### 6. Разница между Controlled и Uncontrolled компонентами?
**Ответ:**

```javascript
// Controlled - React управляет состоянием
const [value, setValue] = useState('');
<input value={value} onChange={(e) => setValue(e.target.value)} />

// Uncontrolled - DOM управляет состоянием
const inputRef = useRef(null);
<input ref={inputRef} defaultValue="initial" />
// значение через inputRef.current.value
```

---

### 7. Что такое Context API?
**Ответ:**
Способ передачи данных через дерево компонентов без prop drilling:

```javascript
const ThemeContext = createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <div>{theme}</div>;
}
```

---

### 8. Что такое Higher-Order Component (HOC)?
**Ответ:**
Функция, принимающая компонент и возвращающая новый компонент:

```javascript
function withAuth(Component) {
  return function AuthenticatedComponent(props) {
    const isAuthenticated = checkAuth();
    if (!isAuthenticated) return <Login />;
    return <Component {...props} />;
  };
}

const ProtectedPage = withAuth(MyPage);
```

---

## 🔄 Redux Toolkit (RTK)

### 1. Зачем нужен Redux?
**Ответ:**
- Централизованное управление состоянием
- Предсказуемые обновления (pure functions)
- Time-travel debugging
- Удобное тестирование
- Для сложных приложений с множеством компонентов

---

### 2. Основные концепции Redux
**Ответ:**

**Store** - централизованное хранилище состояния
```javascript
const store = configureStore({
  reducer: rootReducer
});
```

**Actions** - объекты, описывающие что произошло
```javascript
{ type: 'INCREMENT', payload: 5 }
```

**Reducers** - чистые функции, изменяющие состояние
```javascript
function counterReducer(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    default:
      return state;
  }
}
```

---

### 3. Что такое createSlice?
**Ответ:**
API RTK для создания reducer и actions одновременно:

```javascript
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1; // можно мутировать (Immer)
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    }
  }
});

export const { increment, decrement } = counterSlice.actions;
export default counterSlice.reducer;
```

**Преимущества:**
- Меньше boilerplate кода
- Автоматическая генерация actions
- Можно мутировать state (Immer под капотом)

---

### 4. Что такое createAsyncThunk?
**Ответ:**
Для асинхронных операций:

```javascript
export const fetchUser = createAsyncThunk(
  'user/fetchUser',
  async (userId, { rejectWithValue }) => {
    try {
      const response = await api.getUser(userId);
      return response.data;
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

// В slice
extraReducers: (builder) => {
  builder
    .addCase(fetchUser.pending, (state) => {
      state.loading = true;
    })
    .addCase(fetchUser.fulfilled, (state, action) => {
      state.loading = false;
      state.user = action.payload;
    })
    .addCase(fetchUser.rejected, (state, action) => {
      state.loading = false;
      state.error = action.payload;
    });
}
```

---

### 5. Типизация Redux с TypeScript
**Ответ:**

```typescript
// Типы для store
export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;

// Типизированные хуки
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;

// Типизация slice
interface CounterState {
  value: number;
}

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 } as CounterState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    }
  }
});
```

---

### 6. Что такое селекторы (createSelector)?
**Ответ:**
Мемоизированные селекторы для оптимизации:

```javascript
import { createSelector } from '@reduxjs/toolkit';

const selectTodos = (state) => state.todos.items;
const selectFilter = (state) => state.todos.filter;

export const selectFilteredTodos = createSelector(
  [selectTodos, selectFilter],
  (todos, filter) => {
    if (filter === 'completed') {
      return todos.filter(todo => todo.completed);
    }
    return todos;
  }
);

// Пересчитывается только при изменении todos или filter
```

---

### 7. Middleware в Redux
**Ответ:**
Промежуточное ПО для перехвата actions:

```javascript
const loggerMiddleware = (store) => (next) => (action) => {
  console.log('dispatching', action);
  const result = next(action);
  console.log('next state', store.getState());
  return result;
};

const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(loggerMiddleware)
});
```

---

## 🏗️ Архитектура FLUX

### 1. Что такое FLUX?
**Ответ:**
Архитектурный паттерн для управления состоянием с однонаправленным потоком данных:

```
Action → Dispatcher → Store → View
  ↑                           ↓
  └─────────── Action ────────┘
```

**Компоненты:**
- **Actions** - события/намерения пользователя
- **Dispatcher** - центральный хаб для всех actions
- **Store** - хранилище состояния и бизнес-логики
- **View** - компоненты UI

---

### 2. Redux как реализация FLUX
**Ответ:**
Redux упрощает FLUX:
- Один Store вместо множества
- Reducers вместо множества Stores
- Actions остаются
- Dispatcher встроен в Store

**Поток данных:**
1. Пользователь взаимодействует с View
2. View dispatch Action
3. Reducer обрабатывает Action и обновляет Store
4. Store уведомляет подписчиков
5. View обновляется

---

### 3. Преимущества однонаправленного потока
**Ответ:**
- Предсказуемость - легко отследить изменения
- Отладка - понятно откуда пришло изменение
- Тестируемость - чистые функции
- Масштабируемость - легко добавлять функционал

---

## 💼 Кейсы с работы

### Кейс 1: Оптимизация производительности большого списка
**Ситуация:** Приложение с 10,000+ элементов в списке тормозит

**Решение:**
1. Виртуализация (react-window, react-virtualized)
2. Мемоизация компонентов (React.memo)
3. Ленивая загрузка (lazy loading)
4. Пагинация/бесконечный скролл
5. Оптимизация селекторов в Redux

```javascript
// Виртуализация
import { FixedSizeList } from 'react-window';

function VirtualizedList({ items }) {
  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
    >
      {({ index, style }) => (
        <div style={style}>{items[index]}</div>
      )}
    </FixedSizeList>
  );
}
```

---

### Кейс 2: Управление состоянием загрузки для множественных запросов
**Ситуация:** Нужно отслеживать loading для разных частей UI

**Решение:**
```javascript
// В slice
const fetchDataSlice = createSlice({
  name: 'data',
  initialState: {
    users: { data: [], loading: false, error: null },
    posts: { data: [], loading: false, error: null }
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.users.loading = true;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.users.loading = false;
        state.users.data = action.payload;
      });
  }
});
```

---

### Кейс 3: Синхронизация состояния между вкладками
**Ситуация:** Нужно синхронизировать состояние между вкладками браузера

**Решение:**
```javascript
// Использование BroadcastChannel или localStorage events
useEffect(() => {
  const channel = new BroadcastChannel('app-state');
  
  channel.onmessage = (event) => {
    if (event.data.type === 'STATE_UPDATE') {
      dispatch(updateState(event.data.payload));
    }
  };
  
  return () => channel.close();
}, []);

// При изменении состояния
const handleStateChange = (newState) => {
  dispatch(updateState(newState));
  channel.postMessage({ type: 'STATE_UPDATE', payload: newState });
};
```

---

### Кейс 4: Обработка ошибок в асинхронных операциях
**Ситуация:** Нужна централизованная обработка ошибок

**Решение:**
```javascript
// Middleware для обработки ошибок
const errorMiddleware = (store) => (next) => (action) => {
  if (action.type.endsWith('/rejected')) {
    const error = action.payload || action.error;
    // Логирование, отправка в Sentry, показ уведомления
    console.error('Error:', error);
    showErrorNotification(error.message);
  }
  return next(action);
};
```

---

## 🧩 Задачи на код

### Задача 1: Реализовать debounce
```javascript
function debounce(func, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
}

// Использование
const debouncedSearch = debounce((query) => {
  console.log('Searching:', query);
}, 300);
```

---

### Задача 2: Flatten массив
```javascript
function flatten(arr) {
  return arr.reduce((acc, item) => {
    return acc.concat(Array.isArray(item) ? flatten(item) : item);
  }, []);
}

// Или с flat
arr.flat(Infinity);
```

---

### Задача 3: Deep clone объекта
```javascript
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') return obj;
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof Array) return obj.map(item => deepClone(item));
  
  const cloned = {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloned[key] = deepClone(obj[key]);
    }
  }
  return cloned;
}

// Или
JSON.parse(JSON.stringify(obj)); // но не работает с функциями, Date, undefined
```

---

### Задача 4: Реализовать Promise.all
```javascript
function promiseAll(promises) {
  return new Promise((resolve, reject) => {
    const results = [];
    let completed = 0;
    
    if (promises.length === 0) {
      resolve(results);
      return;
    }
    
    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then(value => {
          results[index] = value;
          completed++;
          if (completed === promises.length) {
            resolve(results);
          }
        })
        .catch(reject);
    });
  });
}
```

---

### Задача 5: Реализовать Redux store
```javascript
function createStore(reducer, initialState) {
  let state = initialState;
  let listeners = [];
  
  const getState = () => state;
  
  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach(listener => listener());
  };
  
  const subscribe = (listener) => {
    listeners.push(listener);
    return () => {
      listeners = listeners.filter(l => l !== listener);
    };
  };
  
  return { getState, dispatch, subscribe };
}
```

---

## 🌐 Общие знания фронтенда

### 1. Что такое CORS?
**Ответ:**
Cross-Origin Resource Sharing - механизм безопасности браузера, ограничивающий запросы между разными доменами.

**Проблема:** Браузер блокирует запросы с `http://localhost:3000` на `http://api.example.com`

**Решение:**
1. **На сервере:** Добавить заголовки
   ```
   Access-Control-Allow-Origin: *
   Access-Control-Allow-Methods: GET, POST, PUT
   Access-Control-Allow-Headers: Content-Type
   ```

2. **Proxy в development:**
   ```json
   // package.json
   "proxy": "http://localhost:5000"
   ```

3. **Preflight запросы:** OPTIONS запрос перед основным

---

### 2. Критический путь рендеринга (Critical Rendering Path)
**Ответ:**
Последовательность шагов браузера для отображения страницы:

1. **HTML** → DOM (Document Object Model)
2. **CSS** → CSSOM (CSS Object Model)
3. **DOM + CSSOM** → Render Tree
4. **Layout** (Reflow) - вычисление позиций элементов
5. **Paint** - отрисовка пикселей
6. **Composite** - композиция слоев

**Оптимизация:**
- Минификация и сжатие ресурсов
- Критический CSS inline
- Отложенная загрузка CSS (non-critical)
- Оптимизация изображений
- Использование `defer` и `async` для скриптов

```html
<!-- Блокирующий -->
<script src="app.js"></script>

<!-- Неблокирующий -->
<script src="app.js" defer></script>
<script src="app.js" async></script>
```

---

### 3. Что такое Event Delegation?
**Ответ:**
Техника обработки событий, при которой обработчик ставится на родительский элемент вместо каждого дочернего:

```javascript
// Плохо - много обработчиков
document.querySelectorAll('.item').forEach(item => {
  item.addEventListener('click', handleClick);
});

// Хорошо - один обработчик
document.querySelector('.list').addEventListener('click', (e) => {
  if (e.target.classList.contains('item')) {
    handleClick(e);
  }
});
```

**Преимущества:**
- Меньше обработчиков в памяти
- Работает с динамически добавленными элементами
- Лучшая производительность

---

### 4. Что такое Webpack и зачем он нужен?
**Ответ:**
Модульный бандлер для JavaScript приложений:

**Функции:**
- Объединение модулей в бандлы
- Транспиляция (Babel, TypeScript)
- Обработка CSS, изображений
- Code splitting
- Tree shaking (удаление неиспользуемого кода)
- Hot Module Replacement (HMR)

**Основные концепции:**
- **Entry** - точка входа
- **Output** - куда сохранять бандлы
- **Loaders** - обработка файлов
- **Plugins** - расширенная функциональность

---

### 5. Что такое Tree Shaking?
**Ответ:**
Удаление неиспользуемого кода из финального бандла:

```javascript
// math.js
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }

// app.js
import { add } from './math';
// subtract будет удален из бандла
```

**Требования:**
- ES6 модули (import/export)
- Настройка в bundler (webpack, rollup)
- Side-effect free код

---

### 6. Что такое Service Worker?
**Ответ:**
Скрипт, работающий в фоне браузера, независимо от веб-страницы:

**Возможности:**
- Кэширование ресурсов (PWA)
- Офлайн работа
- Push уведомления
- Фоновые запросы

```javascript
// Регистрация
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}

// sw.js
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
```

---

### 7. Что такое HTTP/2 и HTTP/3?
**Ответ:**

**HTTP/2:**
- Мультиплексирование (несколько запросов в одном соединении)
- Server Push
- Сжатие заголовков (HPACK)
- Бинарный протокол

**HTTP/3:**
- Использует QUIC (UDP вместо TCP)
- Встроенное шифрование
- Быстрое установление соединения
- Лучше работает при потере пакетов

---

### 8. Что такое XSS и CSRF?
**Ответ:**

**XSS (Cross-Site Scripting):**
Внедрение вредоносного JavaScript кода:

```javascript
// Уязвимость
<div>{userInput}</div> // если userInput = "<script>alert('XSS')</script>"

// Защита
<div>{escapeHtml(userInput)}</div>
// Или в React автоматически экранируется
```

**CSRF (Cross-Site Request Forgery):**
Выполнение действий от имени пользователя:

**Защита:**
- CSRF токены
- SameSite cookies
- Проверка Referer/Origin заголовков

---

### 9. Что такое Bundle Splitting и Code Splitting?
**Ответ:**

**Bundle Splitting:**
Разделение кода на несколько бандлов:
```javascript
// webpack.config.js
optimization: {
  splitChunks: {
    chunks: 'all',
    cacheGroups: {
      vendor: {
        test: /[\\/]node_modules[\\/]/,
        name: 'vendors',
      }
    }
  }
}
```

**Code Splitting (динамический импорт):**
```javascript
// React.lazy
const LazyComponent = React.lazy(() => import('./LazyComponent'));

// Обычный динамический импорт
const module = await import('./module.js');
```

**Преимущества:**
- Меньший initial bundle
- Параллельная загрузка
- Кэширование отдельных частей

---

### 10. Что такое SSR (Server-Side Rendering)?
**Ответ:**
Рендеринг React компонентов на сервере:

**Преимущества:**
- SEO оптимизация
- Быстрый First Contentful Paint
- Работа без JavaScript

**Недостатки:**
- Сложность настройки
- Нагрузка на сервер
- Hydration проблемы

**Решения:**
- Next.js
- Remix
- Gatsby (SSG)

---

## 🧩 Дополнительные задачи на код

### Задача 6: Реализовать throttle
```javascript
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

// Использование
const throttledScroll = throttle(() => {
  console.log('Scrolling');
}, 100);
```

---

### Задача 7: Реализовать curry функцию
```javascript
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      return function(...nextArgs) {
        return curried.apply(this, args.concat(nextArgs));
      };
    }
  };
}

// Использование
const add = (a, b, c) => a + b + c;
const curriedAdd = curry(add);
curriedAdd(1)(2)(3); // 6
curriedAdd(1, 2)(3); // 6
```

---

### Задача 8: Реализовать Promise.race
```javascript
function promiseRace(promises) {
  return new Promise((resolve, reject) => {
    promises.forEach(promise => {
      Promise.resolve(promise)
        .then(resolve)
        .catch(reject);
    });
  });
}
```

---

### Задача 9: Реализовать bind
```javascript
Function.prototype.myBind = function(context, ...args) {
  const fn = this;
  return function(...newArgs) {
    return fn.apply(context, [...args, ...newArgs]);
  };
};

// Использование
const obj = { name: 'John' };
function greet(greeting) {
  return `${greeting}, ${this.name}`;
}
const boundGreet = greet.myBind(obj, 'Hello');
boundGreet(); // "Hello, John"
```

---

### Задача 10: Реализовать функцию compose
```javascript
function compose(...fns) {
  return function(value) {
    return fns.reduceRight((acc, fn) => fn(acc), value);
  };
}

// Или с reduce
function compose(...fns) {
  return (value) => fns.reduceRight((acc, fn) => fn(acc), value);
}

// Использование
const add1 = x => x + 1;
const multiply2 = x => x * 2;
const composeFn = compose(multiply2, add1);
composeFn(5); // (5 + 1) * 2 = 12
```

---

### Задача 11: Реализовать функцию pipe (обратный compose)
```javascript
function pipe(...fns) {
  return (value) => fns.reduce((acc, fn) => fn(acc), value);
}

// Использование
const pipeFn = pipe(add1, multiply2);
pipeFn(5); // (5 + 1) * 2 = 12
```

---

### Задача 12: Найти пересечение массивов
```javascript
function intersection(arr1, arr2) {
  return arr1.filter(item => arr2.includes(item));
}

// Для больших массивов (O(n))
function intersectionOptimized(arr1, arr2) {
  const set = new Set(arr2);
  return arr1.filter(item => set.has(item));
}
```

---

### Задача 13: Реализовать функцию groupBy
```javascript
function groupBy(array, key) {
  return array.reduce((acc, item) => {
    const group = typeof key === 'function' ? key(item) : item[key];
    if (!acc[group]) {
      acc[group] = [];
    }
    acc[group].push(item);
    return acc;
  }, {});
}

// Использование
groupBy([{age: 23}, {age: 24}, {age: 23}], 'age');
// { 23: [{age: 23}, {age: 23}], 24: [{age: 24}] }
```

---

### Задача 14: Реализовать функцию memoize
```javascript
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Использование
const expensiveFn = (n) => {
  console.log('Computing...');
  return n * 2;
};
const memoized = memoize(expensiveFn);
memoized(5); // Computing... 10
memoized(5); // 10 (из кэша)
```

---

## 💼 Дополнительные кейсы с работы

### Кейс 5: Оптимизация ререндеров в большом приложении
**Ситуация:** Компонент ререндерится слишком часто

**Решение:**
```javascript
// 1. Мемоизация компонентов
const ExpensiveComponent = React.memo(({ data }) => {
  // ...
});

// 2. Мемоизация колбэков
const handleClick = useCallback(() => {
  // ...
}, [dependencies]);

// 3. Мемоизация значений
const expensiveValue = useMemo(() => {
  return compute(data);
}, [data]);

// 4. Разделение состояния
// Вместо одного большого объекта
const [user, setUser] = useState({ name: '', email: '', age: 0 });
// Разделить на несколько
const [name, setName] = useState('');
const [email, setEmail] = useState('');
const [age, setAge] = useState(0);
```

---

### Кейс 6: Управление модальными окнами в Redux
**Ситуация:** Нужно управлять множеством модальных окон

**Решение:**
```javascript
// slice
const modalsSlice = createSlice({
  name: 'modals',
  initialState: {
    modals: [] // [{ id: '1', type: 'confirm', props: {} }]
  },
  reducers: {
    openModal: (state, action) => {
      state.modals.push(action.payload);
    },
    closeModal: (state, action) => {
      state.modals = state.modals.filter(m => m.id !== action.payload);
    }
  }
});

// Компонент
function ModalManager() {
  const modals = useAppSelector(state => state.modals.modals);
  
  return (
    <>
      {modals.map(modal => (
        <Modal key={modal.id} {...modal.props} />
      ))}
    </>
  );
}
```

---

### Кейс 7: Оптимистичные обновления в Redux
**Ситуация:** Нужно сразу обновить UI, не дожидаясь ответа сервера

**Решение:**
```javascript
const updatePost = createAsyncThunk(
  'posts/update',
  async (postData, { dispatch, getState }) => {
    // Оптимистичное обновление
    dispatch(postsSlice.actions.updatePostOptimistic(postData));
    
    try {
      const response = await api.updatePost(postData);
      return response.data;
    } catch (error) {
      // Откат при ошибке
      const previousState = getState().posts.items;
      dispatch(postsSlice.actions.rollbackPost(previousState));
      throw error;
    }
  }
);
```

---

### Кейс 8: Реализация undo/redo функциональности
**Ситуация:** Нужна возможность отмены действий

**Решение:**
```javascript
// Middleware для истории
const historyMiddleware = (store) => (next) => (action) => {
  if (action.type.includes('UNDOABLE')) {
    const currentState = store.getState();
    // Сохранить в историю
    history.push(currentState);
  }
  return next(action);
};

// Reducer для истории
const historySlice = createSlice({
  name: 'history',
  initialState: {
    past: [],
    present: null,
    future: []
  },
  reducers: {
    undo: (state) => {
      if (state.past.length > 0) {
        state.future.unshift(state.present);
        state.present = state.past.pop();
      }
    },
    redo: (state) => {
      if (state.future.length > 0) {
        state.past.push(state.present);
        state.present = state.future.shift();
      }
    }
  }
});
```

---

## 🎯 Вопросы на понимание концепций

### 1. Почему Redux использует чистые функции (pure functions)?
**Ответ:**
- Предсказуемость - одинаковый input = одинаковый output
- Тестируемость - легко тестировать
- Time-travel debugging - можно воспроизвести любое состояние
- Отсутствие side effects - нет скрытых зависимостей

---

### 2. Когда использовать Redux, а когда Context API?
**Ответ:**

**Redux:**
- Сложное состояние с множеством компонентов
- Нужен time-travel debugging
- Много асинхронных операций
- Нужны middleware (логирование, аналитика)
- Большая команда (стандартизация)

**Context API:**
- Простое состояние (тема, язык)
- Небольшое приложение
- Редкие обновления
- Не нужны DevTools

---

### 3. Что такое Immutability и зачем она нужна?
**Ответ:**
Неизменяемость данных - создание новых объектов вместо изменения существующих:

```javascript
// Мутация (плохо)
const user = { name: 'John', age: 30 };
user.age = 31; // изменили оригинал

// Иммутабельность (хорошо)
const updatedUser = { ...user, age: 31 }; // новый объект
```

**Зачем:**
- Предсказуемость
- Отслеживание изменений (React, Redux)
- Простое сравнение (shallow comparison)
- Безопасность (нельзя случайно изменить)

---

### 4. Разница между useEffect и useLayoutEffect?
**Ответ:**

**useEffect:**
- Выполняется после рендера и paint
- Асинхронный
- Не блокирует браузер
- Для большинства случаев

**useLayoutEffect:**
- Выполняется после рендера, но до paint
- Синхронный
- Блокирует paint
- Для измерений DOM и предотвращения мерцания

```javascript
// useEffect - может быть мерцание
useEffect(() => {
  setWidth(ref.current.offsetWidth);
}, []);

// useLayoutEffect - без мерцания
useLayoutEffect(() => {
  setWidth(ref.current.offsetWidth);
}, []);
```

---

### 5. Что такое Reconciliation в React?
**Ответ:**
Процесс сравнения нового Virtual DOM со старым и определения минимальных изменений:

**Алгоритм:**
1. Сравнение типов элементов
2. Сравнение ключей (keys)
3. Обновление только измененных частей

**Правила:**
- Ключи должны быть стабильными
- Ключи должны быть уникальными
- Не использовать индекс как ключ (если порядок меняется)

---

## 📝 Типичные вопросы на собеседовании

### 1. Расскажи о своем опыте с Redux
**Структура ответа:**
- Где использовал (проекты)
- Какие проблемы решал
- Какие паттерны применял
- С какими сложностями столкнулся
- Как оптимизировал

---

### 2. Как бы ты организовал структуру Redux в большом проекте?
**Ответ:**
```
src/
  store/
    slices/
      users/
        usersSlice.ts
        usersSelectors.ts
        usersTypes.ts
      posts/
        postsSlice.ts
        postsSelectors.ts
    middleware/
      logger.ts
      errorHandler.ts
    index.ts
    hooks.ts
```

**Принципы:**
- Feature-based структура
- Разделение slice, selectors, types
- Переиспользуемые хуки
- Middleware для общих задач

---

### 3. Как ты тестируешь Redux код?
**Ответ:**
```javascript
// Тест reducer
import reducer from './counterSlice';

test('increment', () => {
  const state = reducer({ value: 0 }, increment());
  expect(state.value).toBe(1);
});

// Тест async thunk
import { fetchUser } from './usersSlice';
import configureStore from '@reduxjs/toolkit';

test('fetchUser success', async () => {
  const store = configureStore({ reducer: usersReducer });
  await store.dispatch(fetchUser(1));
  const state = store.getState();
  expect(state.user).toBeDefined();
});
```

---

## 🎓 Полезные ресурсы для подготовки

1. **Официальная документация:**
   - [Redux Toolkit](https://redux-toolkit.js.org/)
   - [React](https://react.dev/)
   - [TypeScript](https://www.typescriptlang.org/)

2. **Практика:**
   - LeetCode (алгоритмы)
   - Frontend Mentor (проекты)
   - Codewars (задачи)

3. **Статьи:**
   - Redux DevTools
   - React Performance
   - Web Vitals

---

## ✅ Чек-лист перед собеседованием

- [ ] Повторил основные концепции JavaScript
- [ ] Знаю разницу между var, let, const
- [ ] Понимаю Event Loop и асинхронность
- [ ] Могу объяснить замыкания и this
- [ ] Знаю TypeScript основы (типы, интерфейсы, generics)
- [ ] Понимаю жизненный цикл React
- [ ] Знаю все основные хуки
- [ ] Могу объяснить Virtual DOM
- [ ] Понимаю Redux flow (action → reducer → store)
- [ ] Знаю createSlice и createAsyncThunk
- [ ] Могу объяснить FLUX архитектуру
- [ ] Знаю что такое CORS
- [ ] Понимаю критический путь рендеринга
- [ ] Решил несколько задач на код
- [ ] Подготовил примеры из опыта работы

---

**Удачи на собеседовании! 🚀**
