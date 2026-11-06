![Lexicon Logo](https://lexicongruppen.se/media/wi5hphtd/lexicon-logo.svg)

# React Hooks Code Reading Practice

## Understanding `useContext` and `useReducer` Through Real Projects

Your goal is not only to run the projects, but to **read code like a developer**, understand how state management works,
and explain your findings in writing.

1. Reading existing codebases  
2. Understanding architecture  
3. Following state flow  
4. Explaining logic clearly  

---

# 🎯 Learning Objectives

By the end of this project, you should be able to:

* Understand when and why `useContext` is used
* Understand when and why `useReducer` is used
* Read and analyze React components
* Trace data flow between components
* Explain code structure in writing
* Compare two state management approaches

---

# 🛠 Tech Stack

* React
* TypeScript
* Vite
* Tailwind CSS

---

# 📂 Projects Included

## 1️⃣ AdminDashboard → `useContext`

A dashboard UI with dark/light mode theme management using Context API.

## 2️⃣ EventManager → `useReducer`

An event management system using reducer-based state logic.

---

# 🚀 Getting Started

## Install Dependencies

```bash
npm install
```

## Start Development Server

```bash
npm run dev
```

---

# Part 1 — Pre-Reading Questions

Before reading the code, answer these questions briefly **inside this README file** under each question.  

1. What problem does `useContext` solve?  
    **Write your answer:** useContext lets a component read data from a shared Context — a value provided once at the top of the component tree that any component below can access directly, no matter how deeply it is nested.

2. What problem does `useReducer` solve?  
    **Write your answer:** It is similar to setState, but it is for complex state logic. Instead of a value setter, it gives you a value and a dispatch function. The component dispatches an action describing what happened — e.g. { type: 'REGISTER_SUCCESS' } — and one central function, the reducer, decides how the state changes in response.

3. What is the difference between local state and global state?  
    **Write your answer:** local state = specific to a single component, easy to manage, and self-contained. Global state is a centralized resource accessible to many components, essential for shared tasks but more complex to handle.

4. When would `useState` be enough?  
    **Write your answer:** When it is not complex state management. For example to an input field, an on/off switch, or it is a counter. The state is just a number or a word or something simple.

5. Why is code readability important in teams?  
    **Write your answer:** It is important for achieving an effective team work, helping less experienced colleagues to develop, increasing pace of bug fixes, testing.

---

# Project A: AdminDashboard (`useContext`)

Study the files related to:

* ThemeContext
* ThemeProvider
* useTheme
* Header
* Sidebar
* DashboardGrid
* AdminDashboard

---

## Tasks

Write your answers **inside this README file**.  

1. Where is the context created?  
    **Write your answer:** ThemeContext.ts with createContext (we need to provide a default)

2. What data is stored in the context?  
    **Write your answer:** A boolean value (isDark) and handleToggleTheme, a function without parameters, that is responsible for changes

3. What does the Provider do?  
    **Write your answer:** It is not enough creating a context and then tell some components to use them. Until we don't provide it doesn't know where to get the context. This is why we need to create a provider/provide the context.

4. How does theme toggling work?  
    **Write your answer:**  It simply just changes setIsDarkMode to the opposite value (true or false).

5. Why was a custom hook (`useTheme`) created?  
    **Write your answer:** Because this gives the chance to the components (for example Header) can reach and use quickly, simply the data of the theme.

6. Which components consume the context?  
    **Write your answer:** Header, SideBar, DashboardGrid, AdminDashboard

7. How does UI change when dark mode is enabled?  
    **Write your answer:** If the user clicks on the button the handleToggleTheme will run and changes the value of isDarkMode. The page renders again and the UI will change because we used `````` in the className-s with condition, so the appearance of UI will change based on the value of isDarkMode.

8. Why is `useContext` better than prop drilling here?  
    **Write your answer:** prop drilling in general has drawbacks such as it is harder to maintain (for example more tedious to rename props in every intermediate component in the path), components end up cluttered with props they don't care about, and components become harder to reuse or refactor.

---

# Project B: EventManager (`useReducer`)

Study the files related to:

* EventManager
* reducer.ts
* EventForm
* EventList
* EventType

---

## Tasks

Write your answers **inside this README file**.

1. What does the initial state contain?  
    **Write your answer:** The initial state is created by createInitialState(initialEvents) is an object containing five properties:
 - events: An array of EventItem called initialEvents
 - editing: null (no event is currently being edited)
 - query: "" (an empty search string)
 - category: "All" (all categories are selected by default)
 - favoritesOnly: false (shows all events, not just favorites)

2. Why is the state grouped into one object?  
    **Write your answer:** It creates a single source of truth.

3. List all reducer action types.  
    **Write your answer:**
'ADD_EVENT'
'DELETE_EVENT'
'TOGGLE_FAVORITE'
'START_EDIT'
'CANCEL_EDIT'
'UPDATE_EVENT'
'SET_QUERY'
'SET_CATEGORY'
'SET_FAVORITES_ONLY'
'RESET_FILTERS'

4. What happens when `ADD_EVENT` is dispatched?  
    **Write your answer:** The reducer creates a new object and updates the events array: it puts the new event to the beginning.

5. What happens when `DELETE_EVENT` is dispatched?  
    **Write your answer:** The reducer filters the events array and with that it returns a new events array without the item the user wanted to delete (based on the id).

6. How does editing mode work?  
    **Write your answer:** The user clicks on the edit icon on an event card dispatches START_EDIT with that event, setting state.editing to the selected EventItem. EventManager detects editing !== null and that is why it renders <EventForm mode="edit" initialEvent={editing} /> . EventForm uses initialEvent to prefill input fields (title, category, location, date, time). If cancellation happens it dispatches CANCEL_EDIT, and the editing will be reset to null without modifying anything. If the form is submitted then it dispatches UPDATE_EVENT. The new event will replace the old and editing will be set again to null.

7. How does search filtering work?  
    **Write your answer:** Filtering happens dynamically with useMemo hook that calculates the filteredEvents whenever something is changed (e.g.event, query or favoritesOnly).

8. Why is `useMemo` used?  
    **Write your answer:** It works like a cache, so it doesn't recalculate everything every time. e.g. favoriteOnly: it recalculates their number only when events array changes. 

9. Why is `useReducer` better than many `useState`s in this project?  
    **Write your answer:** In this case there would be a lot of separate useState, a lot of props, so with useReducer the code is cleaner.

10. How does `dispatch()` connect UI to logic?  
     **Write your answer:** User click triggers dispatches and the reducer determines how the state should change based on this dispatch type/action type. Based on the new state the site is re-rendered and it shows the new UI.

---

# Part 3 — Compare the Hooks

Complete the table:

| Topic              | useContext                                                                                                                                                                       | useReducer                                      |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|
| Main Purpose       | Context lets the parent component make some information available to any component in the tree below it, no matter how deep it is, without passing it explicitly through proops. | It handles state changes in a clean way.        |
| Best For           | It is a great way to explicitly pipe data through our UI tree to the components that use it.                                                                                     | Several values that always change together      |
| State Type         | Global                                                                                                                                                                           | ?                                               |
| Complexity         | ?                                                                                                                                                                                | ?                                               |
| Example in Project | Dark theme, user logged in                                                                                                                                                       | CRUD event handling e.g.: update existing event |

---

# Optional  Challenges

Choose one or more:

* Save theme in localStorage
* Add event sorting
* Add event validation
* Add pagination
* Split reducer into multiple files
* Add unit tests
* Improve accessibility

---

# Submission

Submit the **completed README.md file** with all answers written inside it.

If you complete optional challenges, also submit your updated project code.

---

# Final Advice

Do not rush.

Read code slowly.  
Trace state carefully.  
Ask “why was this built this way?”  
That question turns beginners into engineers.  

---

# 🚀 Good Luck!

Think deeply, write clearly, and enjoy the process.
