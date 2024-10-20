# ssr-wip

A repo to test and play around ssr with rust. Objective is to create a full-stack web framework like Next.js in Rust.

## Components
This framework would use Function Components like the one made in React. This components would be by default Server Components. This means that they would be rendered on the server at build time and re-validated as needed. 
The idea is that you make a function that exports an HTML fragment that get's created and stored in memory at build time. 

You'll need two kind of components: dynamic and cached. 

### Cached components
The defauld ones, this are components that can query data that doesn't change as much or at all. This components are like product pages, navigation, footer, blog posts, etc. 
All of the things that are maybe updated from time to time or just static. This kind of component would just be created at build time and revalidated on-demand or every x amound of time. This component would just return an HTML that can be sent directly to the client on request.

### Dynamic components
This are components that are always rendered by server after listening to the clinet. 
- The client does a request for a resource (GET) and attaches to it a special header that describes the state of the client session. This header would contain info like is the user logged in or not, what is the session token of the user, what kind of user it is etc
- The server would take the request, parse the header and get the initial state of all of the dynamic components that the user requested. Meanwhile it would do the necessary steps to build the component, like fetching data from a db.
- The client would instantly recieve a piece of readable HTML that would be displayed to the user.
- After the fetch the data would be sent to the client, maybe by using like a second thread, so that the user has a little payload.
- The client would just swap the initial state with the fully loaded one.

### Component state
Having to manage dynamic components you'll need to have a way to manage the state of the component. 
A dynamic component is in either:
- loading state
- error state
- default state
- not found state

To manage this states you'll need to create different exports for a certain component so that the application can send the right one based on the request.
- The client asks for a dynamic component
- The server immediatly sends the loading state
- The client recieves a loading component
- The server fetches the data needed and then sends back the loaded component
- The client swaps the loading state for the default one
- If the server didn't find any data it would have returned the not found state
- If the server encounterd an error it would have returned the error state
