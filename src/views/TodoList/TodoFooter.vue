<template>
  <div class="todo-footer">
    <el-checkbox @change="onSelect" v-model="isSelect">已完成{{ ready }}/全部{{ all }}</el-checkbox>
    <el-button type="text" v-if="isShowBtn" @click="onRemove">清除已完成项目</el-button>
  </div>
</template>

<script>
import {ref} from 'vue';

export default {
  props: {
    all: {
      type: Number,
      required: true
    },
    ready: {
      type: Number,
      required: true
    },
    isShowBtn: {
      type: Boolean,
      required: false,
      default: false
    },
  },
  emits: ['selectAllItem', 'removeAllReady'],
  setup(_, context) {

    const {emit} = context;

    let isSelect = ref(false);

    function onSelect(val) {
      emit('selectAllItem', val);
    }

    function onRemove() {
      emit('removeAllReady');
    }

    return {
      isSelect: isSelect.value,
      onSelect, onRemove
    }
  },
  // created() {
  //   this.isSelect = this.all === this.ready ? this.all !== 0 : false;
  // },
  // updated() {
  //   this.isSelect = this.all === this.ready ? this.all !== 0 : false;
  // },
}
</script>

<style scoped>
.todo-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
  height: 20px;
  color: red;
}

</style>
