<script setup>
import { computed, onMounted, ref, watch } from 'vue';

  const todos =ref([]);//입력한 내용을 배열에 집어넣음
  const name = ref('');

  const input_content=ref(''); //입력 내용
  const input_category=ref(null);

  //입력값 todos에 넣는 함수
  const addTodo = () =>{
    if(input_content.value.trim() === '' || input_category.value === null ) return
    //입력 내용 todos 배열로 저장하고 달라졌으면 로컬스토리지 저장
    todos.value.push({
      content:input_content.value,
      category:input_category.value,
      createAt: new Date().getTime(),
      done:false,
      editable:false
    });

    input_content.value = ''; 
    input_category.value = null;
  }

  //입력한 순서대로 배열 정렬(한번 정렬하고 종료되므로 computed)
  const todos_asc = computed(()=>todos.value.sort((a,b)=>b.createAt - a.createAt))
  //console.log('dddd',todos_asc)


  //이름입력하는 것을 인지하고 로컬스토리지에 저장(입력과 동시에 반영)
  watch(name, (newVal) => {
    localStorage.setItem('name',newVal)
  })

  //todos 배열 변화된것 인지하고 로컬스토리지 업데이트
  watch(todos, (newVal) => {
    localStorage.setItem('todos',JSON.stringify(newVal))
  },{deep:true /* immediate: true */})
  //deep - 속성의 프로퍼티나, 하위 뎁스값도 감시

  //새로열었을때 로컬스토리지에서 불러오기(없을때는 비워놈)
  onMounted(()=>{
    name.value = localStorage.getItem('name') || '';
    todos.value = JSON.parse(localStorage.getItem('todos') || [])
  });

  const removeTodo = (todo) =>{
    todos.value = todos.value.filter((item)=>item !== todo);//내가 선택한 todo item 제외하고 나머지 새 배열
  }


</script>

<template>
  <main class="app">
    <section class="greeting">
      <h1 class="title">
        안녕하세요!
        <input type="text" placeholder="이름을 입력해주세요" v-model="name">님
      </h1>      
    </section>

    <section class="create-todo">
      <h2>To Do List</h2>
      <form id="new-todo-form" @submit.prevent="addTodo">
       <!-- v-on:submit.prevent // form submit 기능 -->
        <h4>오늘의 할 일은 무엇인가요?</h4>
        <input type="text" id="content" placeholder="to do list 작성" v-model="input_content">
        <!-- {{ input_content }} -->
        <div class="options">
          <label>
            <input type="radio" name="category" id="category1" value="business" v-model="input_category">
            <span class="bubble business"></span>
            <div>business</div>
          </label>
          <label>
            <input type="radio" name="category" id="category2" value="personal" v-model="input_category">
            <span class="bubble personal"></span>
            <div>personal</div>
          </label>
        </div>
        <!-- {{ input_category }} -->
        <input type="submit" value="Add todo">
        
      </form>
    </section>

    <section class="todo-list">
      <div v-for="todo in todos_asc" v-bind:class=" `todo-item ${todo.done && 'done'} `">
          <label>
            <input type="checkbox" v-model="todo.done">
            <span v-bind:class="`bubble ${todo.category === 'personal'&& 'personal' }`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content">
            <!-- 화면에서 직접 수정해줘도 로컬스토리지 저장됨(양방향으로 영향) -->
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">  
              Delete
            </button>
          </div>
      </div>
    </section>
  </main>
</template>



