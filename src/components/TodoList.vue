<template lang="html">
  <section class="tasks">
    <div class="container">
      <h2>Список задач</h2>
      <button class="popup-content" @click="showForm">
        Добавить
      </button>
      <form id="addTask">
        <div v-if="isFormShowed" class="modal">
          <h2>Создание задачи</h2>
          <input id="name" v-model="title" type="text" placeholder="Введите название" /><br>
          <textarea v-model="desc" rows="5" placeholder="Введите описание"></textarea><br>
          <input type="button" value="Отмена" v-on:click="hideForm"/>
          <input type="button" value="Готово!" v-on:click="add"/>
        </div>
      </form>
      <div class="tasks__list">
        <div class="tasks__item" v-for="task in tasks" :key="task.id">
          <h3> {{ task.title }} #{{ task.id }}</h3>
          <div class="tasks__desc" v-if="task.desc">
            {{ task.desc }}
          </div>
          <input class="tasks__delete-button"
               type="button"
               value="X"
               @click="del(task.id)"
          >
        </div>
      </div>
      <div class="tasks__have-not" v-if="tasks.length < 1">
        Как то пусто. Чайку?
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'TodoList',

  data() {
    return {
      id: '',
      title: '',
      desc: '',
      isFormShowed: false,
      empty: false,
      tasks: [
        {
          id: '1',
          title: 'Test task 1',
          desc: 'Description1',
        },
        {
          id: '2',
          title: 'Test task 2',
          desc: 'Description2',
        },
      ],
    };
  },

  methods: {
    add: () => {
      if (this.title !== '') {
        const obj = {};
        obj.id = this.tasks.length;
        obj.title = this.title;
        obj.desc = this.desc;
        this.tasks.push(obj);
        this.title = '';
        this.desc = '';
        this.saveTasks();
        this.hideForm();
      }
    },
    del: (ev) => {
      const delIndex = this.tasks.findIndex(x => x.id === ev);
      this.tasks.splice(delIndex, 1);
      this.saveTasks();
      this.checkEmpty();
    },
    showForm: () => {
      this.isFormShowed = true;
    },
    hideForm: () => {
      this.isFormShowed = false;
    },
    checkEmpty: () => {
      if (this.tasks.length < 1) {
        this.empty = true;
      } else {
        this.empty = false;
      }
    },
    saveTasks: () => {
      const parsedTasks = JSON.stringify(this.tasks);
      localStorage.setItem('tasks', parsedTasks);
    },
  },
  mounted() {
    if (localStorage.getItem('tasks')) {
      try {
        this.tasks = JSON.parse(localStorage.getItem('tasks'));
      } catch (e) {
        localStorage.removeItem('tasks');
      }
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
