# ES7 Async and Await

Promises have been the predominant solution to the "callback hell" issue with JavaScript, but starting with ES6, and now ES7 it is slowly being tucked behind the scenes to pave way for `async` and `await`.

Currently, "callback hell" can be mitigated by use of libraries such as [async](https://github.com/caolan/async) or by using generators as popularized by [koa](http://koajs.com/). But they both kind of have been in the realm of hacks/helpers just masking the fundamental issue.

With ES7 we will be able to write our asynchronous code as if it were synchronous in a less hacky way.

> Note that you can already start using modern and future-oriented features by using [babel](babeljs.io)

## Async

Here's an example of async code.

```javascript
async function run(){
    if(Math.round(Math.random())){
        return 'Success!';
    } else {
        throw 'Failure!';
    }
}
```

The `async` keyword allows us to use `await` and it GUARANTEES that the function will return a `Promise` with either the resolved or rejected state. When you return from an async function, you will return a `Promise` object that is resolved with the value. To reject, throw an error, which will return an error object.

The code block above is essentially the same as:

```javascript
function run(){
    if(Math.round(Math.random())){
        return Promise.resolve('Success!');
    }else{
        return Promise.reject('Failure!');
    }
}
```

We can take any function and make it return a `Promise` object by adding `async` keyword. Here is an example:

```javascript
async function get(value){
    return value + 10;
}
```

## Await

Now, the **await** keyword.

While inside the async function, promise-returning expressions can be added with the `await` keyword. By doing so, **it will pause the execution of the rest of the function UNTIL the returned promise has been resolved or rejected**. Check out this simple example:

```javascript
function op(){
    return new Promise(function(resolve,reject){
        setTimeout(function(){
            if(Math.round(Math.random())){
                resolve('Success')
            }else{
                reject('Fail')
            }
        },2000)
    });
}

async function foo(){
    console.log('running')
    try {
        var message = await op();
        console.log(message)
    } catch(e) {
        console.log('Failed!', e);
    }
}

foo()
```

When you call `foo()` it will wait for 2000 ms and either resolve or reject. Notice the try/get. Now we can catch errors.

## Conclusion

In conclusion, this is the best way to solve the "callback hell" without un-catchable promise errors and generator hacks. ES7 is still in the works, but with babel or traceur, it is possible to use it today.
