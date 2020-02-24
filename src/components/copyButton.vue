<template>
  <button @click="copy" :class="{sucess:sucess,error:error}">点击复制</button>
</template>
<script>
export default {
  props:['value'],
  data(){
    return {
      sucess:false,
      error:false,
    }
  },
  methods:{
    //复制
    copy:function(){
      let data=this.value;
      if (window.jsinterface) {
                window.jsinterface.copy(data);
                return true;
            }
            let isSupport = document.queryCommandSupported || document.queryCommandSupported("copy");
            if (!isSupport) return false;
            var textArea = document.createElement("textarea");
            textArea.value = data;
            textArea.style.position = "absolute";
            document.body.appendChild(textArea);
            textArea.focus();
            textArea.setSelectionRange(0, textArea.value.length);
            var successful = false;
            try {
                successful = document.execCommand('copy');
            } catch (err) {
                successful = false;
                this.error=true;
            }
            this.sucess=successful;
            document.body.removeChild(textArea);
            return successful;
    },
  }
}
</script>

<style scoped>
button {
  margin: 0;
  padding: 0;
  border-radius: 5px;
  background-color: #53caff;
  color: #333;
  border: 3px solid transparent;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  outline: none;
  transition: background-color .5s ease-in-out;
  -webkit-transition: background-color .5s ease-in-out;
}
.sucess {
  background-color: #5bb974;
}
</style>