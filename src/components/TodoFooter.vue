<template>
  <footer class="footer">
    <span class="todo-count"
      >剩余<strong>{{ count }}条任务</strong></span
    >
    <ul class="filters">
      <li>
        <a
          :class="{ selected: isSel === 'all' }"
          href="javascript:;"
          @click="change('all')"
          >全部</a
        >
      </li>
      <li>
        <a
          href="javascript:;"
          :class="{ selected: isSel === 'no' }"
          @click="change('no')"
          >未完成</a
        >
      </li>
      <li>
        <a
          href="javascript:;"
          :class="{ selected: isSel === 'yes' }"
          @click="change('yes')"
          >已完成</a
        >
      </li>
    </ul>
    <button class="clear-completed" @click="clear">清除已完成</button>
  </footer>
</template>

<script>
export default {
  props: {
    count: {
      type: Number,
      default: () => 0,
    },
  },
  data() {
    return {
      // all 全部 no 未完成 yes 已完成
      isSel: 'all',
    };
  },
  methods: {
    change(val) {
      this.isSel = val;
      // 把完成状态传给父级
      this.$emit('change', val);
    },
    clear() {
      this.$emit('clear');
    },
  },
};
</script>
