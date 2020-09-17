# Basic usage
css and basic routing and functionality

# Data fetching  branch
Next.js’s pre-rendering feature.
The two forms of pre-rendering: Static Generation and Server-side Rendering.
Static Generation with data, and without data.
getStaticProps and how to use it to import external blog data into the index page.
Some useful information on getStaticProps.

## Pre-rendering.
By default, Next.js pre-renders every page. This means that Next.js generates HTML for each page in advance, instead of having it all done by client-side JavaScript. Pre-rendering can result in better performance and SEO.

Each generated HTML is associated with minimal JavaScript code necessary for that page. When a page is loaded by the browser, its JavaScript code runs and makes the page fully interactive. (This process is called hydration.)
 
### Static Generation with and without Data
Essentially, getStaticProps allows you to tell Next.js: “Hey, this page has some data dependencies — so when you pre-render this page at build time, make sure to resolve them first!”

In lib/posts.js, we’ve implemented getSortedPostsData which fetches data from the file system. But you can fetch the data from other sources, like an external API endpoint, and it’ll work just fine:

You can also query the database directly:

*code*

```
import someDatabaseSDK from 'someDatabaseSDK'

const databaseClient = someDatabaseSDK.createClient(...)

export async function getSortedPostsData() {
  // Instead of the file system,
  // fetch post data from a database
  return databaseClient.query('SELECT posts...')
}
```

# Note: In development mode, getStaticProps runs on each request instead.

### Server side rendering [Fetching Data at Request Time]
If you need to fetch data at request time instead of at build time, you can try Server-side Rendering: On each request data is fetched and HTml is generated. 

# * But it updates the pre-rendering HTML automatically 
To use ***Server-side Rendering***, you need to export ***getServerSideProps*** instead of ***getStaticProps*** from your page.

COde 
```
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    }
  }
}
```

#### Client-side Rendering
If you do not need to pre-render the data, you can also use the following strategy (called Client-side Rendering):

- Statically generate (pre-render) parts of the page that do not require * external data.
- When the page loads, fetch external data from the client using JavaScript and populate the remaining parts.

#### SWR
The team behind Next.js has created a React hook for data fetching called SWR. We highly recommend it if you’re fetching data on the client side. It handles caching, revalidation, focus tracking, refetching on interval, and more. We won’t cover the details here, but here’s an example usage:
```
import useSWR from 'swr'

function Profile() {
  const { data, error } = useSWR('/api/user', fetch)

  if (error) return <div>failed to load</div>
  if (!data) return <div>loading...</div>
  return <div>hello {data.name}!</div>
}
```
