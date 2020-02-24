<template>
<div>
    <textarea placeholder="请输入内容" v-model="word"></textarea>
</div>
</template>

<script>
export default {
  props:['content'],
  data() {
    return {
      word:'',
    }
  },
  activated() {
    this.word=this.content;
    this.$emit("update:tempContent", '');
  },
  watch: {
    word:function(newValue){
      this.$emit("update:content", newValue);
    }
  },
}
</script>

<style>
textarea {
  width: 95%;
  height: 300px;
  font-size: 18px;
}
</style>
