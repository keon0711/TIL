## v-for
- v-for을 사용해 리스트 렌더링이 가능하다.
- `:key="id"`로 todo객체에 바인딩한다.
```
<ul>
  <li v-for="todo in todos" :key="todo.id">
    {{ todo.text }}
  </li>
</ul>
```

## 리스트를 업데이트하는 2가지 방법
1. `todos.value.push(newTodo)`
2. `todos.value = todos.value.filter(/*...*/)`
```
function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
```
