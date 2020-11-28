<template>
  <div id="app">
    <h1>ToDoリスト</h1>
    <input type="radio" id="all" v-model="condition" value="allDisplay" />
    <label>すべて</label>
    <input type="radio" id="doing" v-model="condition" value="doingDisplay" />
    <label>作業中</label>
    <input type="radio" id="finish" v-model="condition" value="finishDisplay" />
    <label>完了</label>
    <br />
    <table>
      <th>ID</th>
      <th>コメント</th>
      <th>状態</th>
      <tbody>
        <tr v-for="(task, index) in showTasks" :key="index">
          <td>{{ task.id }}</td>
          <td>{{ task.comment }}</td>
          <td class="state">
            <button id="status" @click="changeTask(task)">
              {{statusKeyLabel[task.state]}}</button
            >&nbsp;
            <button @click="delTask(task.id)">削除</button>
          </td>
        </tr>
      </tbody>
    </table>
    <h3>新規タスクの追加</h3>
    <input type="text" v-model="inputTaskText" />&nbsp;
    <button v-on:click="addTask">追加</button>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      condition: 'allDisplay',
      inputTaskText: '',
      tasks: [],
      statusCode: { working: 1, complete: 2 },
      statusKeyLabel: { 1: '作業中', 2: '完了' },
    };
  },
  computed: {
    showTasks: function() {
      switch (this.condition) {
        case 'allDisplay':
          return this.tasks;
        case 'doingDisplay':
          return this.tasks.filter(tasks => {
            return tasks.state === this.statusCode.working;
          });
        case 'finishDisplay':
          return this.tasks.filter(tasks => {
            return tasks.state === this.statusCode.complete;
          });
        default:
          return [];
      }
    }
  },
  methods: {
    addTask() {
      const index = this.tasks.length + 1;
      this.tasks.push({
        id: index,
        comment: this.inputTaskText,
        state: this.statusCode.working
      });
      this.inputTaskText = '';
    },
    delTask(delNum) {
      this.tasks.splice(delNum - 1, 1);
      this.tasks.forEach((task, index) => {
        this.$set(this.tasks[index], 'id', index + 1);
      });
    },
    changeTask(tasks) {
      const statusNum = tasks.state === this.statusCode.working ? this.statusCode.complete : this.statusCode.working;
      tasks.state = statusNum;
    },
  }
};
</script>