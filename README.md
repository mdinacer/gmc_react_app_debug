# React Debugging Report

## 1. Project Setup
The sample React application used is a basic to-do list app. It consists of three components: `App`, `TodoList`, and `TodoItem`. The `App` component holds the state of all todos and passes them down as props to `TodoList`, which in turn renders individual `TodoItem` components. React Developer Tools browser extension was installed and used for debugging. The project was set up using `create-react-app` and runs on React 18.

## 2. Initial Observations
After running the application, a few issues were noticeable:
- Clicking the Add Todo button sometimes did not update the list.
- Newly added todos did not display the correct text in the TodoItem component.
- No errors appeared in the console, making it unclear what the exact cause was.

## 3. Using React Developer Tools
I opened the Components tab in React Developer Tools and inspected the state and props of each component:
- The `App` component's state showed that new todos were being added, but the `TodoList` component did not always re-render.
- `TodoItem` sometimes received an undefined `text` prop.
- The props passed from `App` to `TodoList` and then to `TodoItem` were checked to ensure consistency.

## 4. Identifying Issues
**Issue 1: New todos do not always render in the list**
- Diagnosis: In `App`, the todos array was being mutated directly using `push`.
- Root Cause: React state updates require immutable operations to trigger a re-render.

**Issue 2: TodoItem props are undefined for new items**
- Diagnosis: The `TodoList` component was not correctly passing the todo object to `TodoItem`.
- Root Cause: Incorrect mapping in `TodoList` was omitting the `text` field.

## 5. Solutions Implemented
**Fix for Issue 1:**
```javascript
// Before
todos.push(newTodo);
setTodos(todos);

// After
setTodos([...todos, newTodo]);
```
This ensures a new array is created and React triggers a re-render.

**Fix for Issue 2:**
```javascript
// Before
{todos.map(todo => <TodoItem key={todo.id} />)}

// After
{todos.map(todo => <TodoItem key={todo.id} text={todo.text} />)}
```
Now, each TodoItem receives the correct `text` prop.

## 6. Verification
After applying the fixes:
- Clicking Add Todo consistently updates the list.
- Each TodoItem displays the correct text.
- React Developer Tools shows the correct state in `App` and correct props in `TodoItem`.
- Manual testing confirmed all previously observed issues are resolved.

## 7. Lessons Learned
- React state must be updated immutably to trigger re-renders.
- Always check that props are correctly passed down the component tree.
- React Developer Tools is essential for inspecting state and props in real time and can save a lot of time during debugging.

