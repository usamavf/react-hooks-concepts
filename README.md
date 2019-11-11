# react-hooks-concepts
In this we have explained new concepts in react hooks. as In new react hooks now the component is functional not class based

# React-Hooks

Implementation of react-Hooks.

## useState Hook

This hooks replaces the use of states in component with this hook in a functional component.

```
const Counter = () => {
  const [counter, setCounter] = useState(0);
  return (
    <div>
      {counter}
      <button onClick={() => setCounter(counter + 1)} >Increment</button>
    </div>
  )
}

export default Counter;
```

## useEffect Hook

This hook is the new functions which are replacing the old life cycle methods like componentDidMount etc

```
useEffect(didUpdate);
```
Accepts a function that contains imperative, possibly effectful code.

Mutations, subscriptions, timers, logging, and other side effects are not allowed inside the main body of a function component (referred to as React’s render phase). Doing so will lead to confusing bugs and inconsistencies in the UI.

Instead, use useEffect. The function passed to useEffect will run after the render is committed to the screen. Think of effects as an escape hatch from React’s purely functional world into the imperative world.

By default, effects run after every completed render, but you can choose to fire them only when certain values have changed.



Following are some of the examples in which we are replacing the old life cycle methods with the useEffect method.

```
class ExampleComponent extends React.Component {
  componentDidMount() {
    console.log('I am mounted!');
  }
  render() {
    return null;
  }
}
```

with useEffect

```
function Example() {
  useEffect(() => console.log('mounted'), []);
  return null;
}
```

componentDidUpdate()
```
componentDidUpdate() {
  console.log('mounted or updated');
}
```

```
useEffect(() => console.log('mounted or updated'));
```

componentWillUnmount()
```
componentWillUnmount() {
  console.log('will unmount');
}
```

```
useEffect(() => {
  return () => {
    console.log('will unmount');
  }
}, []);
```

In useEffect as we have seen in above examples we are passing a second parameter as empty array. This is because useEffect will run every time whenever any of the data is updated in a component. And in some cases it can become infinite loop so with empty array it will purely behaves are componentDidMount and if we can to run it on specific state or prop then just pass that to the array. As in the bellow example we are just want it to run when the value in count is updated.

```
// component did update
useEffect(() => {
     console.log('will only update if value in count is changed');
}, [count]); // // Only re-run the effect if count changes, we can pass multiple states/props as well
```


## Conditionally firing an effect
The default behavior for effects is to fire the effect after every completed render. That way an effect is always recreated if one of its dependencies changes.

However, this may be overkill in some cases, like the subscription example from the previous section. We don’t need to create a new subscription on every update, only if the source props has changed.

To implement this, pass a second argument to useEffect that is the array of values that the effect depends on. Our updated example now looks like this:

```
useEffect(
  () => {
    const subscription = props.source.subscribe();
    return () => {
      subscription.unsubscribe();
    };
  },
  [props.source],
);
```
Now the subscription will only be recreated when props.source changes.
