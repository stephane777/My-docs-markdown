# NEXT.JS starter project

This is a starter template for [Learn Next.js](https://nextjs.org/learn).

## CREATE A NEXT.JS REACT APP

`npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"`

You now have a new directory called nextjs-blog. Let’s cd into it:

```properties
npm run dev
```

## ROUTING

/pages/index.js is the file root for the app. The page that will be displayed by default.

Every pages for routing needs to be stored at /pages.
The Link component enables client-side navigation. There will be no reload from the browser when the user navigate.
All navigation happen with javascript.

```html
<Link href="/posts/first-post">
    <a>this page!</a>
  </Link>
```

Furthermore, in a production build of Next.js, whenever Link components appear in the browser’s viewport, Next.js automatically prefetches the code for the linked page in the background. By the time you click the link, the code for the destination page will already be loaded in the background, and the page transition will be near-instant!

Next.js automatically optimizes your application for the best performance by code splitting, client-side navigation, and prefetching (in production).

Note: If you need to link to an external page outside the Next.js app, just use an `<a>` tag without Link.

If you need to add attributes like, for example, className, add it to the a tag, not to the Link tag. Here’s an example.

## Assets, metadata and CSS

### Assets

All assets should go to the top-level folder **public**

1. create the /public/images folder
2. add your images inside this /public/images folder

Instead of optimizing images at build time, Next.js optimizes images on-demand, as users request them.
Images are lazy loaded by default. That means your page speed isn't penalized for images outside the viewport.
Images load as they are scrolled into viewport.

```js
import Image from 'next/image';

const YourComponent = () => (
	<Image
		src="/images/profile.jpg" // Route of the image file
		height={144} // Desired size with correct aspect ratio
		width={144} // Desired size with correct aspect ratio
		alt="Your Name"
	/>
);
```

### Metadata

Next provide the **Head** component which is used to dynamically update the html tag head.
For example we can add the Head component to all the route page in our app and change the title tag with the title of the page.

```js
import Head from 'next/page';
export default function FirstPost() {
	return (
		<>
			<Head>
				<title>First Post</title>
			</Head>
			<h1>First Post</h1>
			<h2>
				<Link href="/">
					<a>Back to home</a>
				</Link>
			</h2>
		</>
	);
}
```

### Styling

Next has built-in support for **styled-jsx**. It's a **CSS-in-JS** library.

```js
<style jsx>{`
  …
`}</style>
```

Next has also built-in support for **css** and **sass**.

We can create a wrapper (<Layout />) component which will be common for all pages. The only purpose for this is to cascade the style for all its children.

```js
export default function Layout({ children }) {
	return <div>{children}</div>;
}
```

Next code splitting feature works on **Css modules** as well. It ensures the minimal amount of CSS is loaded for each page!

### Global styling

CSS Modules are useful for component-level styles.
But if you want some CSS to be loaded by every page, Next.js has support for that as well.

To load global CSS files, **create a file called pages/\_app.js** with the following content:

```js
export default function App({ Component, pageProps }) {
	return <Component {...pageProps} />;
}
```

You need to restart the development server when you add pages/\_app.js. Press Ctrl + c to stop the server and run:

```properties
npm run dev
```

At this point we can now create a styles/global.css file a root level then import it and use it in the pages/\_app.js

### Pre-rendering and Data Fetching

By default, Next.js pre-renders every page. This means that Next.js generates HTML for each page in advance, instead of having it all done by client-side JavaScript. Pre-rendering can result in better performance and SEO.

Each generated HTML is associated with minimal JavaScript code necessary for that page. When a page is loaded by the browser, its JavaScript code runs and makes the page fully interactive. (This process is called hydration.)

Next.js has two forms of pre-rendering: Static Generation and Server-side Rendering. The difference is in when it generates the HTML for a page.

1. Static Generation is the pre-rendering method that generates the HTML at build time. The pre-rendered HTML is then reused on each request.
2. Server-side Rendering is the pre-rendering method that generates the HTML on each request.

> We recommend using Static Generation (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

### Static Generation with Data using getStaticProps

While exporting the component that needs to fetch data, access the file system or querying a db you can export getStaticProps with async.

1. getStaticProps runs at build time in production.
2. Inside this function you can fetch external data and return it as props:

Essentially, getStaticProps allows you to tell Next.js:

> “Hey, this page has some data dependencies — so when you pre-render this page at build time, make sure to resolve them first!”
