# ES7 Async and Await

Promises have been the current solution to the "callback hell" but starting from ES6, and now ES7 it is being tucked behind the scenes (while still being a major foundation) to pave way for `async` and `await`.

Currently, "callback hell" can be mitigated by use of libraries such as [async](https://github.com/caolan/async) or by using generators as popularized by [koa](http://koajs.com/). But they both kind of have been in the realm of hacks/helpers.

With ES7 we will be able to write our asynchronous code as if it were synchronous.

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

The `async` keyword allows us to use `await` and it GUARANTEES that the function will return a `Promise`. When you return from an async function, you will return a `Promise` object that is resolved with the value. To reject, throw an error, which will return an error object.

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

While inside the async function, on expressions that return a promise, you can add in the `await` keyword and **it will pause the execution of the rest of the function UNTIL the returned promise has been resolved or rejected**. Check out this example:

```javascript
function op(){
    return new Promise(function(resolve,reject){
        setTimeout(function(){
            if(Math.round(Math.random())){
                resolve('Success')
            }else{
                reject('Fail')
            }
        },1000)
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

When you call `foo()` it will wait for 1000 ms and either resolve or reject. Notice the try/get. Now we can catch errors.

## Conclusion

In conclusion, this is the best way to solve the "callback hell" without un-catchable promise errors and generator hacks. ES7 is still in the works, but with babel or traceur, it is possible to use it today.
