---
title: 'Writing The Server using Hyper Library'
series: 'hff'
description: "Writing the server and routing system for the HTMX fullstack framework. Using hyper library to write the server. Planing file-based routing."
date: "3:03AM,5-11-2023"
tags: ['programming', 'recreational', 'htmx', 'htmx-framework', 'fullstack', 'devlog', 'filebasedrouting', 'hyper', 'http']
---

Today, I'll write the server and configure the filebased routing to some extent.   
Also I forgot to censor r*st in my last blogpost which was dangerous. Git pushed the fix. Also I guess it is fine since "drama driven development" ain't too bad either.

### The Hyper HTTP Library and the Reason   
Hyper is a low level HTTP library. I could've used Axum or any other higher level library I'm familiar with but I guess I have to use Hyper for obvious reasons. HTMX is heavily dependent on serverside computation so setting up proper endpoints is the first priority. For one full page there needs to two types of endpoint, one for HTMX to request data from and smoothly swap the page content as well as update the document uri. Another type is needed handle GET requests from browsers or SEO bots for rendered html. And it can get extremely complicated and tedious to set up endpoints using available backend frameworks.

### Example Time   
```rs
// todo.rs

#[server(AddTodo, "/api")]
pub async fn add_todo(title: String) -> Result<(), ServerFnError> {
    let mut conn = db().await?;

    match sqlx::query("INSERT INTO todos (title, completed) VALUES ($1, false)")
        .bind(title)
        .execute(&mut conn)
        .await
    {
        Ok(_row) => Ok(()),
        Err(e) => Err(ServerFnError::ServerError(e.to_string())),
    }
}

#[component]
pub fn BusyButton() -> impl IntoView {
    view! {
        <button on:click=move |_| {
            spawn_local(async {
                add_todo("So much to do!".to_string()).await;
            });
        }>
            "Add Todo"
        </button>
    }
}
```   
This one is straight out of [Leptos Documentation](https://leptos-rs.github.io/leptos/server/25_server_functions.html).   

TBC...