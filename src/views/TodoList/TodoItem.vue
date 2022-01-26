<template>
  <div class="todo-item">
    <el-checkbox :label="todoData" v-show="!data.isEdit"/>
    <el-input v-model="data.name" placeholder="请输入内容" v-show="data.isEdit" @change="changeName"/>
    <div class="item-right">
      <el-icon @click="setEdit" class="edit-item-icon" :size="20">
        <edit/>
      </el-icon>
      <el-button type="text" class="remove-btn" @click="onRemove">删除</el-button>
    </div>
  </div>
</template>

<script>
import {reactive} from "vue";
import {Edit} from '@element-plus/icons-vue'

export default {
  props: {
    todoData: {
      type: String,
      required: true
    }
  },
  emits: ['removeItem', 'changeTodoName'],
  components: {Edit},
  setup(props, context) {

    const {emit} = context;

    let data = reactive({
      isEdit: false,
      name: props.todoData
    });

    function onRemove() {
      emit('removeItem');
    }

    function setEdit() {
      data.isEdit = true;
    }

    function changeName(val) {
      emit('changeTodoName', val);
      data.isEdit = false;
    }

    return {
      data, onRemove, setEdit, changeName
    }
  },
}
</script>

<style scoped lang="scss">
.todo-item {
  display: flex;
  justify-content: space-between;
  height: 30px;
  align-items: center;
  padding: 8px;
  background: #42b983;
  margin: 8px 0;

  .item-right {
    display: flex;
    align-items: center;
  }

  &:hover {
    .remove-btn {
      display: block;
    }

    .edit-item-icon {
      display: block;
    }
  }

  .remove-btn {
    display: none;
    transition: display ease-in-out 0.3s;
    color: red;
    margin-left: 4px;
  }

  .edit-item-icon {
    display: none;
  }
}
</style>
