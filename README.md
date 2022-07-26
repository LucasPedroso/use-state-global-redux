# useStateGlobalRedux

Custom Hooks para compartilhar estado global com Redux

Sendo para estados simples que não necessitariam de criar actions e toda a complexidade que redux trás.

Counter.tsx
```typescript
export function Counter() {
  const [state, setState] = useStateGlobalRedux(Main, { count: 0 });

  return (
    <>
      <Header />
      <div>
        <p>Você clicou {state.count} vezes</p>
        <Button
          label="Page Count Counter++"
          onClick={() => setState({ count: state.count + 1 })}
        />
      </div>
    </>
  );
}
```

OtherComponent.tsx
```typescript
export function OtherComponent() {
  const [state, setState] = useStateGlobalRedux(Main);

  return (
    <>
      <Header />

      <div>
        <p>Você clicou {state.count} vezes</p>
        <Button
          label="Page OtherComponent Counter++"
          onClick={() => setState({ count: state.count + 1 })}
        />
      </div>
    </>
  );
}
}
```

main.ts
``` 
...
import { reducer } from 'use-state-global-redux'

const store = createStore(reducer, composeWithDevTools(applyMiddleware(thunk)));
...
```

## Para usá-lo com outros reducers necessita de uma configuração extra

main.ts

``` typescript
...
import { reducer } from 'use-state-global-redux'

const reducers = combineReducers({
  noConfig: reducer,  // noConfig pode ser qualquer nome
  example: anotherReducer
});

const store = createStore(reducer, composeWithDevTools(applyMiddleware(thunk)));
...
```
Counter.tsx
```typescript
export function Counter() {
  const [state, setState] = useStateGlobalRedux({ name: 'counter' }, { count: 0 }, { nameReducer: 'noConfig' });

  return (
    <>
      <Header />
      <div>
        <p>Você clicou {state.count} vezes</p>
        <Button
          label="Page Count Counter++"
          onClick={() => setState({ count: state.count + 1 })}
        />
      </div>
    </>
  );
}
```

OtherComponent.tsx
```typescript
export function OtherComponent() {
  const [state, setState] = useStateGlobalRedux({ name: 'counter' }, {}, { nameReducer: 'noConfig' });

  return (
    <>
      <Header />

      <div>
        <p>Você clicou {state.count} vezes</p>
        <Button
          label="Page OtherComponent Counter++"
          onClick={() => setState({ count: state.count + 1 })}
        />
      </div>
    </>
  );
}
}
```

