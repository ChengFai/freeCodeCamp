---
id: 5895f70df9fc0f352b528e68
title: Стратегії автентифікації
challengeType: 2
forumTopicId: 301547
dashedName: authentication-strategies
---

# --description--

Стратегія - це процес авторизації користувача. Ви можете вибрати стратегію, яка дозволяє користувачам авторизуватись на основі збережених даних (якщо ви їх вказували) або від різних провайдерів таких, як Google або GitHub. For this project, we will use Passport middleware. Passport provides a comprehensive set of strategies that support authentication using a username and password, GitHub, Google, and more.

`passport-local@~1.0.0` has already been added as a dependency. Add it to your server as follows:

```javascript
const LocalStrategy = require('passport-local');
```

Tell passport to **use** an instantiated `LocalStrategy` object with a few settings defined. Make sure this (as well as everything from this point on) is encapsulated in the database connection since it relies on it!:

```javascript
passport.use(new LocalStrategy((username, password, done) => {
  myDataBase.findOne({ username: username }, (err, user) => {
    console.log(`User ${username} attempted to log in.`);
    if (err) return done(err);
    if (!user) return done(null, false);
    if (password !== user.password) return done(null, false);
    return done(null, user);
  });
}));
```

This is defining the process to use when you try to authenticate someone locally. First, it tries to find a user in your database with the username entered. Then, it checks for the password to match. Finally, if no errors have popped up that you checked for (e.g. an incorrect password), the `user` object is returned and they are authenticated.

Many strategies are set up using different settings. Generally, it is easy to set it up based on the README in that strategy's repository. A good example of this is the GitHub strategy where you don't need to worry about a username or password because the user will be sent to GitHub's auth page to authenticate. As long as they are logged in and agree then GitHub returns their profile for you to use.

In the next step, you will set up how to actually call the authentication strategy to validate a user based on form data.

Відправте свою сторінку коли впевнились, що все правильно. Якщо виникають помилки, ви можете <a href="https://forum.freecodecamp.org/t/advanced-node-and-express/567135#authentication-strategies-6" target="_blank" rel="noopener noreferrer nofollow">переглянути проєкт, виконаний до цього етапу</a>.

# --hints--

Passport-local повинен бути залежністю.

```js
async (getUserInput) => {
  const url = new URL("/_api/package.json", getUserInput("url"));
  const res = await fetch(url);
  const packJson = await res.json();
  assert.property(
    packJson.dependencies,
    'passport-local',
    'Your project should list "passport-local " as a dependency'
  );
}
```

Passport-local should be correctly required and set up.

```js
async (getUserInput) => {
  const url = new URL("/_api/server.js", getUserInput("url"));
  const res = await fetch(url);
  const data = await res.text();
  assert.match(
    data,
    /require.*("|')passport-local("|')/,
    'You should have required passport-local'
  );
  assert.match(
    data,
    /new LocalStrategy/,
    'You should have told passport to use a new strategy'
  );
  assert.match(
    data,
    /findOne/,
    'Your new local strategy should use the findOne query to find a username based on the inputs'
  );
}
```

# --solutions--

```js
/**
  Backend challenges don't need solutions, 
  because they would need to be tested against a full working project. 
  Please check our contributing guidelines to learn more.
*/
```
