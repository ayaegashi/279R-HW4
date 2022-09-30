# 279R HW4: 

## Reflection

#### What are the significant software concepts that this combination of technologies has that each previous set of technologies did not? Or that they handle significantly differently?

Svelte is a Javascript framework. However, unlike other frameworks such as React (used for my Doodle and When2Meet implementations) which operates as a Javascript library, Svelte is a frontend compiler. This makes Svelte faster. In React, each time props or state is changed the entire component is updated. However, Svelte variables only react to changes made to themselves, which makes its performance faster since it needs to update fewer items.

From the developer's perspective, I also found Svelte very intuitive to work with. Files read like HTML files with the logic of the app contained inside a `<script>` tag. This is then compiled into Javascript (similar to how JSX in React turns HTML tags into Javascript). Thus, with a few exceptions for syntax (i.e. `on:click` vs. `onClick`), the move to Svelte from a traditional HTML, CSS, and Javascript combination is not very difficult. In addition, working with Svelte felt more organized since the HTML, CSS, and Javascript (which would normally be organized into their own files) were combined neatly into one file. ObservableHQ similarly combines all aspects of a web app into one scrollable page or notebook. However, since ObservableHQ does not actually integrate the different languages together as Svelte does, the ObservableHQ notebook becomes long and difficult to navigate, while a Svelte document is more concise and readable. 

Using Firebase felt very similar to MongoDB. Both allowed the same functionality of storing user input across browser sessions. I read online that Firebase is more suited for smaller applications (such as this todo-app), while MongoDB is better suited for high-performance use cases. I found the Firebase interface easier to navigate because they had more documentation support, but otherwise felt that the two technologies were very similar.


## Citations

I followed this five-part tutorial on Youtube to build my todo app: https://youtu.be/AV04qN6frmE

Research on Svelte: 

https://www.turing.com/blog/svelte-vs-react-which-javascript-framework-should-you-choose-in-2022/#:~:text=Svelte%20is%20remarkably%20faster%20than,not%20rely%20on%20VDOM%20diffing.

https://medium.com/cacher-app/svelte-is-really-fast-45224f57bd86

Research on Firebase:

https://javascript.plainenglish.io/firebase-or-mongodb-for-your-next-app-d2d6575b0714


## How to Open my To-Do List App

Run `npm install` to install necessary dependencies. Then, to start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

<!-- ## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment. -->
