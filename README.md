# Unit 6 Lesson 1 Practice: Promises

## Directions
Fork and clone this lab. Respond to questions in clear, concise sentences directly within this markdown file.

**1. What will the following code snippet log? Why?**
  ```javascript
  console.log('Line 1')
  setTimeout(() => console.log('Line 2'), 1000)
  console.log('Line 3')
  ```
  + This snippet logs `Line 1`, then `Line 3`, and finally `Line 2` after one second. This is because `setTimeout` is an asynchronous function that lets JavaScript move on to the next line of code while it waits for the a certain amount of time as passed before executing its callback function.

**2. What does the following code snippet log? Why?**
  ```javascript
  function createPromise(seconds) {
    return new Promise((resolve) => {
      setTimeout(function() {
        resolve(`After ${seconds} second(s), this promise is resolved.`)
      }, seconds * 1000)
    })
  }

  console.log(createPromise(1))
  ```
  + This snippet does not log text to the console. This is because `createPromise` returns a promise object. The Promise object is resolved after `setTimeout` runs, but we have not programmed it to do anything once that promise _is_ resolved.

**3. How do we use our `createPromise` function to log `"After 1 second(s), this promise is resolved."`**
  + In order to log this statement to the console, we can add a `.then()` statement to our promise that receives the data passed by `resolve` and can log it.

```js
createPromise(1).then(resolve => console.log(resolve))
```

**4. What does the following code snippet return? What does it log?**
  ```javascript
  const ourPromise = new Promise((resolve) => {
    resolve(12);
  })

  ourPromise.then(value => value * 2);
  ```
  + The code returns a Promise object. It does not log anything, but the returned Promise object has a fulfilled value of `24`. If we wanted to log `24`, we would chain on another `.then`:
 
```js
ourPromise.then(value => value * 2).then(value => console.log(value));
```

**5. What does the last line of code return? What does it log?** <br> **Note: Instead of using the `Promise` constructor to create a Promise that immediately resolves to 12, we can just use the [`Promise.resolve`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve) method.**
  ```javascript
  const ourPromise = Promise.resolve(12);
  ourPromise.then(value => value * 2).then(value => value + 10);
  ```
  + No number is logged. This finale line of code returns a Promise object with a fullfilled value of `34`. Promise chainging occurs because `.then()` always returns another Promise object. Therefore, we can keep manipulating the resolved value of one promise by minpulating it in the next promise. In that case, the first resolved value of 12, get multipled by 2 in the second Promise and then 10 is added in the last Promise. 

**6. What does the following code snippet return? What does it log? How does this differ from the question above?**
  ```javascript
  Promise.resolve(12).then(value => value * 2).then(value => console.log(value + 10))
  ```
  + In the finale `.then`, the number `34` is logged, but since nothing is being _returned_ in the last callback function, the resultsing Promise has a fullfilled value of `undefined`.  

**7. What does the following code snippet return? What does it log? How does this differ from the question above?**
  ```javascript
  Promise.resolve(12).then(value => value * 2).then(value => {
    console.log(value + 10);
    return value + 10;
    });
  ```
  + This code snippet logs `34` **and** returns a Promise with a fullfilled value of `34`. 

**8. What does the following code snippet return? What does it log? How does this differ from the question above?**
  ```javascript
  Promise.reject(12).then(value => value * 2).then(value => {
    console.log(value + 10);
    return value + 10;
    }).catch(reason => {
      const message = `${reason} is a bad number.`;
      console.error(message);
      return reason;
    });
  ```
  + This snippet throws an error, `12 is a bad number` and returns a Promise with a fullfilled value of `12`. `Promise.reject` returns a rejected promise, meaning our `catch()` method is invoked _instead_ of our `.then()` methods.  This `catch` statement interpolates `12` into a string and logs it to the console as an error.
