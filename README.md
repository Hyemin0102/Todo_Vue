<img width="50%" src="https://github.com/Hyemin0102/Todo_React/assets/128768462/bdba1a4e-77b4-4812-80ad-e5e6865b7d89">

# Todo_Vue
vue를 활용한 Todo 앱

## 🔎프로젝트 소개
https://charming-sherbet-f2cc91.netlify.app/

처음으로 vue를 사용해 만든 간단한 Todo 앱입니다.

todo list 추가, 삭제, 체크 기능을 구현하였으며, 로컬스토리지를 활용해 새로고침시에도 데이터가 유지됩니다.


## ⏳개발 기간
2023.07.10 ~ 2023.07.14 (5일)

## ⚙개발 환경
Vue

## 🚩주요 기능
* Todo List 추가, 삭제 시 v-model 양방향 데이터 바인딩으로 로컬스토리지 동시 관리(watch, onMounted 사용)
* 완료 여부에 따라 다른 스타일 적용

## 📌코드 리뷰
사용자의 이름과 할일 입력하는 값을 v-model로 양방향 데이터 바인딩 가능하도록 하고, watch로 해당 데이터를 감시 -> 변화 시 로컬스토리지에 바로 넣기
```javascript
 watch(name, (newVal) => {
    localStorage.setItem('name',newVal)
  });
watch(todos, (newVal) => {
    localStorage.setItem('todos',JSON.stringify(newVal))
  },{deep:true /* immediate: true */})
```
페이지가 로드되었을 때 로컬스토리지에 있는 값 불러와서 화면 출력
```javascript
 onMounted(()=>{
    name.value = localStorage.getItem('name') || '';
    todos.value = JSON.parse(localStorage.getItem('todos') || [])
  });
```
v-bind로 클라스에 접근하고,  todo.done이 true이면 done 클래스 추가시킨다.
그리고 v-model로 todo.done이라는 속성과 체크박스를 양방향 바인딩한다.
즉, todo.done이 true면 체크박스가 선택된 상태이고, false면 체크박스가 해재된 상태다.
input tyle="checked" 인 경우 기본은 false고 체크 시 true로 불리언값을 반환시킨다.
```javascript
<div v-for="todo in todos_asc" v-bind:class=" `todo-item ${todo.done && 'done'} `">
  <label>
    <input type="checkbox" v-model="todo.done">
    <span v-bind:class="`bubble ${todo.category === 'personal'&& 'personal' }`"></span>
  </label>
```

## 😊프로젝트를 마치며
* 여러가지 디렉티브를 사용해 간단하게 todo 기능을 구현해보았는데 html에 직접 개입할 수 있어 다루기가 쉬웠고 직관적으로 보여져 편리했다.
* 다음엔 조금 더 기능이 많이 들어간 vue 웹페이지를 구현해봐야겠다. vue만의 css 라이브러리 등을 사용해 봐야겠다.
