// 选择特效字体输入时 的显示部分
<template>
  <div class="awesome">
    <span>粗体</span>
    <input type="checkbox" value="bold" v-model="checked">
    <span>斜体</span>
    <input type="checkbox" value="italic" v-model="checked">
    <span>随机颜色</span>
    <input type="checkbox" value="color" v-model="checked">
    <span>字号:</span>
    <input type="number" id="number" value="0" placeholder="0表示默认大小"
    step="2" v-model="fontSize">
    <br>
    <span>开始颜色:</span>
    <input type="text" v-model="sColor"><colorZone :color='sColorParsed'></colorZone>
    <br>
    <span>色相偏移：</span>
    <input type="number" value="360" placeholder="一个循环为360"
    step="90" v-model="rotate">
    <br>
    <div v-if="rotate==''">
      <span>结束颜色:</span>
      <input type="text" v-model="eColor"><colorZone :color='eColorParsed'></colorZone>
    </div>
    <span>超链接:</span>
    <input type="text" v-model="url">
    <br>
    <textarea placeholder="输入内容" v-model="awesomeInput" class="awesomeInput"></textarea>
    <button @click="add" class="submit" type="button">确认添加</button>
    <br>
  </div>
</template>

<script>
import Color from 'color'
import colorZone from './colorZone'
window.Color=Color;
export default {
  props:['content'],
  components:{
    colorZone,
  },
  data() {
    return {
      checked:[],
      each:[],
      awesomeInput:'',
      url:'',
      fontSize:0,
      sColor:'',
      sColorParsed:'',
      sColorArr:[],
      eColor:'',
      eColorParsed:'',
      eColorArr:[],
      rotate:'',
      img:'',
    }
  },
  activated() {
    this.$emit("update:tempContent",this.formatedInput);
  },
  methods: {
    addImg:function(){
      if(this.img){
        this.$emit("update:content",this.content+this.addTag('img',this.img));
      }
    },
    clear:function(){
      /*this.checked=[];
      this.url='';
      this.sColor='';
      this.eColor='';
      this.fontSize=0;*/
      this.awesomeInput='';
      
    },
    addTag:function(tag,word,tagValue){
      return `[${tag}${tagValue ? '='+tagValue:''}]${word}[/${tag}]`;
    },
    add:function() {
      this.$emit("update:content",this.content+this.formatedInput);
      this.clear();
    },
    format:function(arr,word){
      return [arr,word];
    },
    randomColor:function(){
      let c=Color({
        r:this.randomNum(0,255),
        g:this.randomNum(0,255),
        b:this.randomNum(0,255),
      });
      return c.hex();
    },
    randomNum:function(minNum, maxNum) {
      return parseInt(Math.random() * ( maxNum - minNum + 1 ) + minNum, 10);
    } 
  },
  computed: {
    //测试输入格式是否正确
    formatedInput:function(){
      let result=this.awesomeInput;
      if(this.eColorParsed!='' && this.sColorParsed!=''){
        let render=Color;
        let s=this.sColorArr,e=this.eColorArr,len=result.length;
        let dr=e[0]-s[0],dg=e[1]-s[1],db=e[2]-s[2];
        result=result.split('').map((x,i)=>
          this.addTag('color',x,render.rgb(
            s[0]+dr*(i+1)/len,s[1]+dg*(i+1)/len,s[2]+db*(i+1)/len).hex())
          ).join('');
      }
      else if(this.sColorParsed!='' && this.rotate!=''){
        let render=Color;
        let s=this.sColorArr,len=result.length,r=parseInt(this.rotate);
        let sColor=render.rgb(s);
        result=result.split('').map((x,i)=>
          this.addTag('color',x,sColor.rotate((i/len*r)).hex()))
          .join('');
      }
      else if(this.checked.indexOf('color')>=0){
        result=result.split('').map(s=>this.addTag('color',s,this.randomColor())).join('');
      }
      //window.console.log(result);
      if(this.fontSize!=0)result=this.addTag('size',result,this.fontSize);
      if(this.url)result=this.addTag('url',result,this.url);
      if(this.checked.indexOf('bold')>=0)result=this.addTag('b',result);
      if(this.checked.indexOf('italic')>=0)result=this.addTag('i',result);
      return result;
    }
  },
  watch: {
    sColor:function(now){
      try {
        let color=Color(now);
        this.sColorParsed=color.hex();
        this.sColorArr=color.rgb().array();
        this.$emit("update:tempContent",this.formatedInput);
      }
      catch(err){
        this.sColorParsed='';
        this.$emit("update:tempContent",this.formatedInput);
      }
    },
    eColor:function(now){
      try {
        let color=Color(now);
        this.eColorParsed=color.hex();
        this.eColorArr=color.rgb().array();
        this.$emit("update:tempContent",this.formatedInput);
      }
      catch(err){
        this.eColorParsed='';
        this.$emit("update:tempContent",this.formatedInput);
      }
    },
    fontSize:function(){
       this.$emit("update:tempContent",this.formatedInput);
    },
    checked:function(){
      this.$emit("update:tempContent",this.formatedInput);
    },
    awesomeInput:function(){
      this.$emit("update:tempContent",this.formatedInput);
    },
    url:function(){
      this.$emit("update:tempContent",this.formatedInput);
    },
    rotate:function(){
      this.$emit("update:tempContent",this.formatedInput);
    },
  },
}
</script>
<style scoped lang='css'>
.awesomeInput {
  height: 100px;
}
.awesome {
  font-size: 18px;
  line-height: 35px;
}
input[type=text] {
  position: relative;
  line-height: 18px;
}

input[type=checkbox] {
    width:15px;
    height:15px; 
    text-align: center;
    margin-right: 10px;
}
input[type=checkbox]:after {
  position: relative;
    width: 100%;
    height: 100%;
    top: 0px;
    content: " ";
    background-color:#eee;
    display: inline-block;
    visibility: visible;
    border-radius: 2px;
    border:1px solid black;
}
input[type=checkbox][disabled=disabled]:after {
    width: 100%;
    height: 100%;
    top: 0;
    content: " ";
    background-color:#DCDCDC;
    color: #000000;
    display: inline-block;
    visibility: visible;
    border-radius: 2px;
    border:3px solid #A9A9A9;
    box-shadow: 0 0 5px -5px black;
}
input[type=checkbox]:checked::before {
  position: absolute;
  z-index: 100;
  top: 0px;
  content: "\2713";/*UNICODE中对号*/
  color: #137333;
  font-size: 22px;
  font-weight:bold;
}

span {
  line-height: 18px;
}
#number{
  width: 100px;
}
.submit {
  border-radius: 5px;
  color:  #137333;
  width: 100px;
  margin: 10px;
  padding: 10px;
  font-size: 18px;
}
.submit:hover {
  background-color: #53caff;
  color: black;
}

</style>