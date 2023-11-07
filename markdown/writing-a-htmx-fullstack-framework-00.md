---
title: 'Writing A HTMX Fullstack Framework From Scratch'
series: 'hff'
description: "begining of a journey to write a htmx fullstack framework from scratch in r*st programming language with little to zero experience. purpose is to make htmx more aproachable to those who are still stuck in 'virtual dom did mount' hell loop and also make it as close to plug n play as possible."
date: '9:31PM,4-11-2023'
tags: ['programming', 'recreational', 'htmx', 'htmx-framework', 'fullstack', 'devlog']
---

We all know what HTMX is by now. And we also know what MDD is.   
But even though there're enough memes, there's no fullstack HTMX framwork. Which is a bit of shame and dissapointing to a hardcore meme driven developer like me.

Let's write one then. What's the end goal? Proper file/component based routing and one single binary to deploy to production that offer Native UI like fast n smooth navigation. And in the future support for serverless computation like AWS lambda to break off monolithic structure and cost reduction.

### The Glorius Beginning of Pure Chaos   
I myself am in love with NEXT.JS. But the frontend framework it is based around is not something I like. Why? 'No Reason.'   
I tried out svelte, fell in love. Hated sveltekit, left.

So I found this situation to be quite amusing cause it gave me a reason to try making a fullstack framework which would provide me a plenty of content to write about and it is quite the opportunity to level up my programming skill thousand folds.

### The Initial Plan and Vision   
Imagine a function that renders out 'hello mom' in a heading 1 element.

How would you aproach this in NEXT?   
```ts
function Random_Component() {
    let data = "mom";

    return (
        <h1>
            {`hello ${ data }`}
        </h1>
    )
}
```   
Right?

Now think of a r*st function like this.   
```rs
fn random_component() -> Html<String> {
    let data = "mom".to_string();

    let body = html!`
        <h1>
            hello {{ data }}
        </h1>
    `;

    body
}
```   
Does not seem too bad in my inexperienced eyes though.

Yeah now let me be clear this is not by any means well thought out or final structure but it gets done the job of getting the idea onto the page. Also while we're at it let's make a bit of change in the function.   
```rs
fn random_component() -> Html<String> {
    let data = "mom".to_string();

    let body = xdom!`       // change html macro to xdom
        <h1>
            hello {{ data }}
        </h1>
    `;

    body
}
```   
We need the x-factor in this day n age. And "X" stands for "G" in rune so it rounds up to 'GDOM.' It's alternative of 'VDOM' and 'der meme factor.'

This just about takes care of one of the main hardle to tackle and now it's time for one of the bigger problems...

### Setting up HX endpoints   
For now after reading the docs and hate comments on twitter it seems the biggest hardle of HTMX is setting up the data endpoints. Which must be resolved as efficiently as possible so users must not have to set up the endpoints. Let me show you an example,   
```rs
async fn render_templates(route: String, hx: bool) -> String {
    let layout = std::fs::read_to_string("path to layout").unwrap();
    // layout.html
    ///
    // <h2> hello { children } </h2>

    let page = std::fs::read_to_string("path to page").unwrap();
    // page.html
    ///
    // <em> mom </em>

    let body: String;

    if route == "/" {
        if !hx {
            body = layout.replace("{ children }", &page)
        } else {
            body = page
        }
    } else {
        body = "Not Found".to_string()
    }

    let mut tera = Tera::default();
    tera.add_raw_template("layout", &body).unwrap();

    let mut ctx = Context::new();
    tera
        .render("layout", &ctx)
        .unwrap()
    // renders out as a String
    ///
    // <h2> hello <em> mom </em> </h2>
}
```   

Now the router is set up to handle the request and call the 'render_templates' accordingly. And if the hx is true then only data is sent not the whole rendered page. This was a makeshift way I'm currently using in my blog website to render html, and it is strictly make shift. Cause we can all see the glaring problem with this solution, the 'else if hell.'   
Now I'm currently thinking of a function that'd list all the hx-calls in the xdom! macro and the data that'd be called. And assigning unique hx-endpoints for all the calls that can happen. At the end we compile all the doms and endpoints and that should do it. So at the end we should be left we too types of endpoint in our server, 'HX-endpoints' for handling hx-calls and headers and 'Route-Handlers' to handle route navigation as the name suggests.

To end today's blog I'll discuss the biggest feature/problem to solve is to write a minimal fetch api like in NEXT.js.

### Fetch api client for Serverside data fetching   
What is next.js infamous for and also the reason I fell in love with it? Ease of data handling (fetch, cache, revalidate).   
So I must have it!   
Which is easier said than done. And I unironically decided to use 'Hyper' as the http library.

Anyways that's as far as primary level 'whiteboard masturbation' goes. So many steps to cover, so many lines of codes to be written. This is the journey of a lifetime, that's what I'd like to call it. This is the first ever proper project I'll be working on so there's bound to be mistakes and a lots to learn. Sounds like fun to me.

That is all for today. May you have a great day or night wherever or whenever you're reading this.   
#### Thank you!! & Goodnight
