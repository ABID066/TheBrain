#### Conditional Rendering Using Ternary Operator

```js
{marks > 80 ? <h1>Brilliant Result</h1> : <h1>Average Result</h1>}
```

#### Conditional Rendering Using Immediately Invoked Function Expressions

```js
{(() => {
    if (status)
        return <button onClick={props.childBtnClick}>lOGoUT FORM HERO</button>
    else
        return <button onClick={props.childBtnClick}>lOGin to HERO</button>
})()}

```


#### Conditional Rendering Using Logical AND (`&&`) Operator

```js
{status && <button>LogOUT From ContactForm</button>}
```


#### Conditional Rendering Using `if-else` Statements

```js
const LoginStatusBtn = (status) => {
    if (status) {
        return <button onClick={() => { alert("ok") }}>Logout Btn</button>
    } else {
        return <button onClick={() => { alert("ok") }}>Login Btn</button>
    }
}
```

#### Conditional Rendering Using `switch` Statements

```js
switch (status) {
    case true:
        return <button>Logout bTN from FOOTER</button>
    case false:
        return <button>Login bTN from FOOTER</button>
    default:
        return null;
}
```