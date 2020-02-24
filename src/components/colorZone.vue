//在框中输入颜色 后面显示颜色 色块
<script>
export default {
  props:['color'],
  render(h) {
    return h('span',{
      class:'color',
      style:{
        backgroundColor:this.color || '#ffffff',
        width:'15px',
        height:'8px',
        display:'inline-block',
        margin:'0 5px',
        verticalAlign:'middle',
      }
    },this.$slots.default)
  },
}
</script>