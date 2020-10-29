<style lang="less">
</style>
<template>
  <div>
    <Select
      placeholder="请输入进行搜索"
      clearable
      filterable
      multiple
      label-in-value
      v-model.lazy="valueText"
      :loading="loading"
      @on-query-change="onQueryChange"
      @on-change="onChange"
    >
      <Option
        style="display: none"
        v-for="(item, index) in defaultOption"
        :value="item.value"
        :key="index"
        >{{ item.label }}
      </Option>
      <Option
        v-for="(item, index) in list"
        :value="item[config.value]"
        :key="index"
        >{{ item[config.label] }}
      </Option>
    </Select>
  </div>
</template>

<script>
/*
config:{
  label:"",
  value:"",
}
defaultOption:[{
  label:"",
  value:""
}]
*/
export default {
  name: "mutiSearchSelect",
  props: {
    value: undefined, //使用.sync绑定
    defaultOption: Array,
    list: Array,
    config: Object,
    remoteMethod: Function,
  },
  data() {
    return {
      loading: false,
      valueText: this.value,
      updateText: false,
    };
  },
  methods: {
    onChange(selection) {
      this.$emit("on-change", selection);
      this.$emit("update:value", selection);
      this.updateText = true;
      setTimeout(() => {
        this.updateText = false;
      }, 0);
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
