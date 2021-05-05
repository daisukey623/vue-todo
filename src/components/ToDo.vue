<template>
  <div>
    <h2>ToDoリスト</h2>
    <form>
      <input
        type="radio"
        name="status"
        value="all"
        checked
        @click="showAll"
      />すべて
      <input
        type="radio"
        name="status"
        value="active"
        @click="showWorking"
      />作業中
      <input
        type="radio"
        name="status"
        value="completed"
        @click="showDone"
      />完了
    </form>

    <table>
      <tr>
        <th>ID</th>
        <th>作業中</th>
        <th>状態</th>
      </tr>
      <tr
        v-for="todo in todos"
        :key="todo.num"
        :class="{
          displayActive: todo.isDisplayActive,
          displayNone: todo.isDisplayNone,
        }"
      >
        <td>{{ todo.id }}</td>
        <td>{{ todo.comment }}</td>
        <td>
          <button @click="changeStatus(todo)">{{ todo.status }}</button>
        </td>
        <td>
          <button @click="deleteTask(todo)">{{ todo.delete }}</button>
        </td>
      </tr>
    </table>

    <h3>新規タスクの追加</h3>
    <form>
      <input type="text" v-model="todo.comment" />
      <input type="button" value="追加" @click="addTask" />
    </form>
  </div>
</template>

<script>
export default {
  name: "ToDo",
  props: {
    msg: String,
  },
  data() {
    return {
      todo: {
        id: 0,
        comment: "",
        status: "作業中",
        delete: "削除",
        isDisplayActive: true,
        isDisplayNone: false,
      },
      todos: [],
    };
  },
  computed: {},
  methods: {
    addTask: function () {
      // コメントが空白なら追加されない
      if (this.todo.comment === "") {
        return;
      }
      console.log(this.todos);
      // dataと紐付け
      const todo = {
        id: this.todo.id,
        comment: this.todo.comment,
        status: this.todo.status,
        delete: this.todo.delete,
        isDisplayActive: true,
        isDisplayNone: false,
      };

      // 次の新規タスクの準備
      this.todo.id++;
      this.todos.push(todo);
      this.todo.comment = "";
    },
    deleteTask: function (todo) {
      const index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
      let idIndex = 0;

      // IDを振り直し
      for (let i = 0; i < this.todos.length; i++) {
        this.todos[i].id = idIndex;
        idIndex++;
      }

      //  追加タスクのIDを連番にする
      this.todo.id = idIndex;
    },
    changeStatus: function (todo) {
      if (todo.status === "作業中") {
        todo.status = "完了";
      } else {
        todo.status = "作業中";
      }
    },
    showAll: function () {
      for (let i = 0; i < this.todos.length; i++) {
        if (
          this.todos[i].status === "完了" ||
          this.todos[i].status === "作業中"
        ) {
          this.todos[i].isDisplayActive = true;
          this.todos[i].isDisplayNone = false;
        }
      }
    },
    showWorking: function () {
      for (let i = 0; i < this.todos.length; i++) {
        if (this.todos[i].status === "完了") {
          this.todos[i].isDisplayActive = false;
          this.todos[i].isDisplayNone = true;
        } else {
          this.todos[i].isDisplayActive = true;
          this.todos[i].isDisplayNone = false;
        }
      }
    },
    showDone: function () {
      for (let i = 0; i < this.todos.length; i++) {
        if (this.todos[i].status === "作業中") {
          this.todos[i].isDisplayActive = false;
          this.todos[i].isDisplayNone = true;
        } else {
          this.todos[i].isDisplayActive = true;
          this.todos[i].isDisplayNone = false;
        }
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.displayActive {
  display: table-row;
}

.displayNone {
  display: none;
}
h3 {
  margin: 40px 0 0;
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
