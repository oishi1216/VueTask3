<template>
  <div id="app">
    <h1>TODOリスト</h1>
    <label><input type="radio" v-model="sort" value="すべて">すべて</label>
    <label><input type="radio" v-model="sort" value="作業中">作業中</label>
    <label><input type="radio" v-model="sort" value="完了">完了</label>
    <table id="tasklist">
      <thead>
        <tr>
          <th>ID</th>
          <th>コメント</th>
          <th>状態</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in searchTaskList" :value="item" :key="item.id">
          <td>{{item.id}}</td>
          <td>{{item.task}}</td>
          <td><button @click="changeStatus(item)">{{item.working}}</button></td>
          <td><button @click="deleteRow(index)">{{item.delete}}</button></td>
        </tr>
      </tbody>
    </table>
    <h2>新規タスクの追加</h2>
    <input type="text" id="taskText">
    <input type="button" value="追加" @click="addTaskList">
  </div>
</template>

<script>
export default {
  data() {
    return {
      item: '',
      sort: 'すべて',
      taskList: []
    }
  },
  methods: {
    addTaskList() {
      let text = document.getElementById("taskText");
      this.taskList.push({id:Object.keys(this.taskList).length, task:text.value, working:'作業中', delete:'削除'})
      text.value = '';
    },
    deleteRow(index) {
      this.$delete(this.taskList, index);
      for (let i=0; i<Object.keys(this.taskList).length; i++){
        this.taskList[i].id = i;
      }
    },
    changeStatus(item) {
      if (item.working === '作業中'){
        item.working = '完了';
      } else {
        item.working = '作業中';
      }
    },
  },
  computed: {
    searchTaskList: function(){
      let sortState = this.sort;
      return this.taskList.filter(function(value){
          if (sortState === 'すべて'){
            return true;
          }
          else if (sortState === '作業中'){
            return value.working === '作業中'
          }
          else if (sortState === '完了'){
            return value.working === '完了'
          }
      })
    }
  },
}
</script>
<style lang="scss">

</style>
