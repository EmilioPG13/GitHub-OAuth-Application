# Set up GitHub OAuth Application

1. Navigate to your GitHub "Settings" page by clicking on your GitHub icon.

![GitHub Settings](https://static-assets.codecademy.com/content/paths/web-security/oauth2/settings.png)

2. Select "Developer settings" in the sidebar.

![Developer Settings](https://static-assets.codecademy.com/content/paths/web-security/oauth2/developer_settings.png)

3. Select "OAuth Apps" in the sidebar of the Developer settings page. Click on the "Register a new application" button.

![OAuth Apps](https://static-assets.codecademy.com/content/paths/web-security/oauth2/OAuthApp.png)

4. Fill out the form with the following information:
   - Application Name: OAuth Project (or any name for your project)
   - Homepage URL: `http://localhost:3000`
   - Authorization Callback URL: `http://localhost:3000/auth/github/callback`

5. Click "Register application".

![Register Application](https://static-assets.codecademy.com/content/paths/web-security/oauth2/Register.png)

6. Take note of the Client ID. Click on "Generate a new client secret" to generate a Client secret. Copy both your Client ID and the Client Secret and paste them into a text file somewhere secure. We will be using these in our application. Note that the secret key is displayed only once—when you navigate away from the GitHub page, you will no longer be able to see the Client Secret.

![Client ID and Secret](https://static-assets.codecademy.com/content/paths/web-security/oauth2/NewClient.png)

## Running Node.js App Locally

If you don't have Node.js installed on your computer, check out this [Setting Up Node Locally](https://www.codecademy.com/articles/setting-up-node-locally) article for instructions.

Once you have Node.js installed, open a terminal/Bash window and navigate to the folder containing the starting code of this project using the `cd` command. 

Once you are in the working directory, run:

```
npm install
```

The above command will install all necessary dependencies for your web application.

# Project Instructions

## Part 1 — GitHub Client ID and Client Secret Set Up

1. Begin by adding the Client ID and Client Secret generated from GitHub's OAuth App.
2. In the **starting-code/** directory, open the **.env** file. Set `GITHUB_CLIENT_ID` to your GitHub Client ID and `GITHUB_CLIENT_SECRET` to your Client Secret. If the **.env** file is not visible, adjust your file explorer settings to show hidden files.

## Part 2 — Express Session Set Up

1. At the top of **app.js**, in the "Package Imports" section, import the [`express-session`](https://github.com/expressjs/session) module into a variable named `session` using `require()`.
2. In the "Express Project Setup" section, initialize the session by calling `session()` within `app.use()`. Pass an object to `session()` with `secret: 'codecademy'`, `resave: false`, and `saveUnitialized: false`.

## Part 3 — Passport Configuration

1. In the "Package Imports" section of **app.js**, import the [`passport`](http://www.passportjs.org/) module into a variable named `passport` using `require()`.
2. Import `Strategy` from `passport-github2` into a variable named `GitHubStrategy`.
3. In the "Passport Configurations" section, use `passport.use()` to configure the GitHub strategy. Pass a new `GitHubStrategy()` instance with a JSON object containing `clientId`, `clientSecret`, and `callbackURL`.
4. Add a verify callback function as the second argument to `GitHubStrategy`. It should take `accessToken`, `refreshToken`, `profile`, and `done` as parameters. Return the profile using `done(null, profile)`.
5. In the "Express Project Setup" section, initialize Passport with `app.use(passport.initialize())`.
6. Configure Passport Session with `app.use(passport.session())`.

## Part 4 — Passport Session Serializers

1. Implement `passport.serializeUser()` with a callback function taking `user` and `done`. Call `done(null, user)` in the callback.
2. Implement `passport.deserializeUser()` similarly, calling `done(null, user)` in its callback.

## Part 5 — Implement OAuth Routes

1. Create a new route `/auth/github` using `app.get()`. Use `passport.authenticate('github', { scope: ['user'] })` as middleware.
2. Implement the callback route `/auth/github/callback` using `app.get()`. Use `passport.authenticate('github', { failureRedirect: '/login', successRedirect: '/' })` as middleware.
3. Protect the `/account` route by adding `ensureAuthenticated` as middleware before the rendering callback.
4. Define `ensureAuthenticated(req, res, next)` at the bottom of **app.js**. Check `req.isAuthenticated()`. If true, call `next()`; otherwise, redirect to `/login`.

# Review

Restart the server and log in to the app using your GitHub account. When you are logged in, you can go to the `/account` page and see details from your GitHub account displayed.

In this project, we implemented OAuth using the `express-session` module to manage user sessions authenticated using the `passport` module. We configured `passport` using the credentials from GitHub and implemented the URL routes for authentication. Finally, we implemented a middleware function to protect the route.