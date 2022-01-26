<template>
  <el-card class="todo-list">
    <header>事件的发布与订阅，组件间的通信</header>
    <div class="todo-head">
      <el-input v-model="data.input" placeholder="请输入待办项" class="search-input" :clearable="true" @change="addItem"/>
      <el-button type="primary" @click="resetData" size="medium">重置数据</el-button>
    </div>
    <el-checkbox-group v-model="data.allSelect">
      <div v-for="(item,index) in currentList" :key="index">
        <transition name="removeItem" appear>
          <TodoItem :todoData="item" @removeItem="()=>remove(index)"
                    @changeTodoName="(name)=>changeTodoName(index,name)"/>
        </transition>
      </div>
    </el-checkbox-group>
    <div v-show="currentList.length ===0">暂无数据</div>
    <hr>
    <TodoFooter :all="data.list.length" :ready="data.allSelect.length" :is-show-btn="!!data.allSelect.length"
                @removeAllReady="()=>remove(-1)"/>
  </el-card>
</template>

<script>
import TodoItem from "./TodoItem";
import TodoFooter from "./TodoFooter";
import {reactive, computed} from 'vue';

let allTodoKey = '@@ALL_TODO';
let allReadyKey = '@@ALL_READY';
let initialList = ['写代码', '喝水', '接水', '上厕所'];

export default {
  components: {TodoItem, TodoFooter},
  setup() {
    let data = reactive({
      list: initialList,
      input: '',
      allSelect: [],
      isSelectAll: false
    });

    function remove(index) {
      if (index === -1) {
        let list = data.list.filter(it => !this.allSelect.includes(it));
        data.list = [...list];
        return;
      }
      data.list.splice(index, 1);
    }

    function select(val, index, name) {
      if (val) {
        return data.allSelect.push(name);
      }
      return data.allSelect.splice(index, 1);
    }

    function selectAll(val) {
      data.allSelect = val ? [...data.list] : [];
    }

    function addItem(val) {
      val && data.list.push(val);
    }

    function resetData() {
      data.list = initialList;
      data.allSelect = [];
    }

    function changeTodoName(index, name) {
      data.list.splice(index, 1, name);
    }

    let currentList = computed(() => {
      if (data.input) {
        let list = data.list.filter(it => it.includes(data.input));
        return [...list];
      }
      return data.list;
    });

    return {
      data, currentList,
      remove, selectAll, select, changeTodoName, addItem, resetData
    };
  },
  // created() {
  //   let todo = window.sessionStorage.getItem(allTodoKey) ?? initialList;
  //   let ready = window.sessionStorage.getItem(allReadyKey) ?? [this.allSelect];
  //   this.list = typeof todo === 'string' ? [...JSON.parse(todo)] : todo;
  //   this.allSelect = typeof ready === 'string' ? [...JSON.parse(ready)] : ready;
  //   this.$eventBus.$on('selectAllItem', this.selectAll);
  // },
  watch: {
    list(val) {
      let list = this.allSelect.filter(it => val.includes(it));
      this.allSelect = [...list];
      window.sessionStorage.setItem(allTodoKey, JSON.stringify(val));
    },
    allSelect(val) {
      window.sessionStorage.setItem(allReadyKey, JSON.stringify(val));
    },
    isSelectAll(val) {
      this.allSelect = val ? [...this.list] : [];
    }
  }
}
</script>

<style scoped lang="scss">
.todo-list {
  width: 45%;

  .todo-head {
    display: flex;
    align-items: center;
    margin: 16px 0;

    button {
      margin-left: 8px;
    }
  }

  //.removeItem-enter-active, .removeItem-leave-active {
  //  transition: ease-in-out 1s;
  //}
  //
  //.removeItem-enter, .removeItem-leave-to {
  //  transform: translateX(-100%);
  //}
  //
  //.removeItem-enter-to, .removeItem-leave {
  //  transform: translateX(0);
  //}
}
</style>
