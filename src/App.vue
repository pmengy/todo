<template>
  <div>
    <section class="todoapp">
      <TodoHeader @add="add" @checked="checked"></TodoHeader>
      <!-- 把计算好的 showList 传入 TodoMain 组件 -->
      <TodoMain :list="showList" @del="del"></TodoMain>
      <TodoFooter :count="count" @change="change" @clear="clear"></TodoFooter>
    </section>
  </div>
</template>

<script>
// 引入 组件
import TodoHeader from './components/TodoHeader.vue';
import TodoMain from './components/TodoMain.vue';
import TodoFooter from './components/TodoFooter.vue';
export default {
  name: 'todoApp',
  data() {
    return {
      list: JSON.parse(localStorage.getItem('list')) || [],
      getSel: 'all',
    };
  },
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter,
  },
  methods: {
    add(val) {
      const id = this.list[this.list.length - 1]
        ? this.list[this.list.length - 1].id + 1
        : 100;
      this.list.push({ name: val, isDone: false, id });
    },
    del(id) {
      const index = this.list.findIndex((ele) => ele.id === id);
      this.list.splice(index, 1);
    },
    change(val) {
      // 把子组件传过来的选中状态放到data里面
      this.getSel = val;
    },
    clear() {
      this.list.forEach((ele) => (ele.isDone = false));
    },
    checked(val) {
      this.list.forEach((ele) => (ele.isDone = val));
    },
  },
  computed: {
    count() {
      return this.list.filter((ele) => !ele.isDone).length;
    },
    showList() {
      // 根据data里的选中状态来渲染数据
      if (this.getSel === 'no') {
        return this.list.filter((ele) => !ele.isDone);
      } else if (this.getSel === 'yes') {
        return this.list.filter((ele) => ele.isDone);
      } else {
        return this.list;
      }
    },
  },
  watch: {
    list: {
      deep: true,
      handler(val) {
        localStorage.setItem('list', JSON.stringify(val || []));
      },
    },
  },
};
</script>
