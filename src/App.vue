<template>
  <div id="app">
    <h1>ToDoリスト</h1>
    <input
      type="radio"
      id="all"
      v-model="condition"
      value="allDisplay"
      v-on:change="changeDisplay(condition)"
    />
    <label>すべて</label>
    <input
      type="radio"
      id="doing"
      v-model="condition"
      value="doingDisplay"
      v-on:change="changeDisplay(condition)"
    />
    <label>作業中</label>
    <input
      type="radio"
      id="finish"
      v-model="condition"
      value="finishDisplay"
      v-on:change="changeDisplay(condition)"
    />
    <label>完了</label>
    <br />
    <table>
      <th>ID</th>
      <th>コメント</th>
      <th>状態</th>
      <tbody>
        <tr v-for="task in tasks" :key="task.id" v-show="task.flag">
          <td>{{ task.id }}</td>
          <td>{{ task.comment }}</td>
          <td class="state">
            <button id="status" @click="changeTask(task)">
              {{ status[task.state].label }}</button
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
      buttonId: '',
      index: 0,
      tasks: [],
      delIndex: 0,
      status: [{ label: '作業中' }, { label: '完了' }]
    };
  },
  methods: {
    changeDisplay(conditionValue) {
      if (conditionValue === 'allDisplay') {
        for (this.index = 0; this.index < this.tasks.length; this.index++) {
          this.tasks[this.index].flag = true;
        }
      } else if (conditionValue === 'doingDisplay') {
        for (this.index = 0; this.index < this.tasks.length; this.index++) {
          if (this.tasks[this.index].state !== 0) {
            (this.tasks[this.index].flag = false);
          } else {
            this.tasks[this.index].flag = true;
          }
        }
      } else {
        for (this.index = 0; this.index < this.tasks.length; this.index++) {
          if (this.tasks[this.index].state !== 1) {
            this.tasks[this.index].flag = false;
          } else {
            this.tasks[this.index].flag = true;
          }
        }
      }
    },
    addTask() {
      this.index = this.tasks.length + 1;
      if (this.condition !== "finishDisplay") {
        this.tasks.push({
          id: this.index,
          comment: this.inputTaskText,
          state: 0,
          flag: true
        });
      } else {
        this.tasks.push({
          id: this.index,
          comment: this.inputTaskText,
          state: 0,
          flag: false
        });
      }
      this.inputTaskText = "";
    },
    delTask(delNum) {
      this.tasks.splice(delNum - 1, 1);
      for (
        this.delIndex = delNum - 1;
        this.delIndex < this.tasks.length;
        this.delIndex++
      ) {
        this.$set(this.tasks[this.delIndex], "id", this.delIndex + 1);
      }
    },
    changeTask(tasks) {
      tasks.state = !tasks.state ? 1 : 0;
    }
  }
};
</script>