---
title: "Manage an application state using localStorage API and Database"
date: 2022-04-14 12:00 +0900
categories: js database
---

# Manage an application state using localStorage API and Database

Good afternoon :)

JavaScript's localStorage API is very useful to save your database resources by making it unnecessary to update and get data everytime there's a change.
However, you may find only relying on localStorage can make it harder to manage changes to the application state made by your server.
For instance, you updated the application that requires changes to users' localStorage data.
Here's a pattern.

```{js}
async function manageState(currentState) {
  let existingState = loadApplicationState();
  if (existingState?.version !== currentState.version) {
    // use whatever key(s)
    saveApplicationState({ version: currentState.version });
  }
}

function loadApplicationState() {
  return JSON.parse(localStorage.getItem("state")); // the key under which you store the state information
}

function saveApplicationState(data) {
  localStorage.setItem("state", JSON.stringify(data));
}
``
