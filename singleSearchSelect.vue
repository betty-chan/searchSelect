<style lang="less">
</style>
<template>
  <div>
    <Select
      placeholder="请输入进行搜索"
      clearable
      filterable
      style="width: 300px"
      v-model.lazy="valueText"
      :loading="loading"
      @on-select="onSelect"
      @on-clear="onClear"
      @on-query-change="onQueryChange"
    >
      <Option style="display: none" v-if="valueText" :value="valueText">{{
        label
      }}</Option>
      <Option
        v-for="(item, index) in list"
        :value="item[config.value]"
        :key="index"
        >{{ item[config.label] }}</Option
      >
    </Select>
  </div>
</template>

<script>
/*
config:{
  label:"",
  value:"",
}
*/
export default {
  name: "singleSearchSelect",
  props: {
    value: undefined, //使用.sync绑定
    label: undefined, //使用.sync绑定
    list: Array,
    config: Object,
    remoteMethod: Function,
  },
  watch: {
    value: function () {
      this.updateText = true;
      this.valueText = this.value;
      setTimeout(() => {
        this.updateText = false;
      }, 0);
    },
  },
  data() {
    return {
      loading: false,
      valueText: this.value,
      updateText: false, //避免不必要的onQueryChange触发
    };
  },
  methods: {
    onSelect(selection) {
      this.$emit("update:value", selection.value);
      this.$emit("update:label", selection.label);
    },
    onClear() {
      this.$emit("update:value", null);
      this.$emit("update:label", null);
    },
    onQueryChange(query) {
      if (this.updateText) {
        return;
      }
      this.remoteMethod(query);
    },
  },
};
</script>
