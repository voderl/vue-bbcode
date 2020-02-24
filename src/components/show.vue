<script>
export default {
    name:'show',
    render:function(h) {
      let word=this.word;
      return h('div',this.cal(h,word));
  },
  //正则匹配 [i][/i] [b][/b] [color=#xxxxxx][/color] 以及可以嵌套
  created() {
    this.regex1=/\n|\[(\w+?)(?:=(.+?))?\]([\s\S]*?)\[\/\1\]/m;
  },
  props:['word'],
  methods: {
    cal(h,word){
      let el=[];
      let regex1=this.regex1;
      //匹配 设置500是因为vue 不让用while 它太害怕卡死了
      for(let i=0;i<500;i++){
        let result=word.match(regex1);
        if(!result)break;
        let now=word.slice(0,result.index+result[0].length);
        if(result.index)el.push(now.slice(0,result.index));
        el.push(this.generate(h,result[1],result[3],result[2],result[0]));
        word=word.slice(result.index+result[0].length);
      }
      el.push(word);
      return el;
    },
    //具体的生成元素
    generate(h,tag,content,tagValue,all){
      if(all=='\n')return h('br');
      content=this.cal(h,content);
      switch(tag){
        case 'color': return h('font',{
          attrs:{
            color: tagValue,
          }
        },content);
        case 'img': return h('img',{
          attrs:{src:content},
          style:'max-height:200px',
        });
        case 'url':
          if(tagValue)return h('a',{
            attrs:{href:tagValue,class:'link',target:'_blank',title:tagValue}
          },content);
          return h('a',{
            attrs:{
              href:content,
              class:'link',
              target:'_blank',
              title:content
            }
          },content);
        case 'size':
          return h('font',{
            attrs:{size:tagValue},
          },content);
        case 'b':
          return h('b',content); 
        case 'i': return h('i',content);
        case 'bilibili':
          return h('iframe',{
            attrs:{
              src:content,
              scrolling:'no',
              border:'0',
              frameborder:'no',
              framespacing:'0',
              allowfullscreen:'true',
              style:'max-width: 100%; width: 544px; height: 415px;',
              width:544,
              height:415,
            }
          });
        default: 
          return;
      }
    }
  },

}
</script>