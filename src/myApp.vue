顶层结构   tagBar  预览区域
内容由content（输入栏中的字体） 和 tempContent（在特效字体输入里 临时进行显示的字体，只有确认加入后才能真正加入content）
<template>
<div>
  <div class="tabBar">
  <button
    v-for="(tab,index) in tabs"
    v-bind:key="index"
    v-bind:class="['tab-button', { active: currentIndex === index },
    {tabLeft:index===0},{tabRight:index===tabs.length-1}]"
    v-on:click="currentIndex = index"
  >{{ tab }}</button>
  </div>
    <transition name="change" mode="out-in">
      <keep-alive>
      <component
        v-bind:is="currentTab"
        :class="currentTab"
        class="tab"
        :content.sync="content"
        :tempContent.sync='tempContent'
      ></component>
       </keep-alive>
    </transition>
 
  <hr>
  <div>
    <p style="font-size: 14px; margin-left: 0px">
      <a style="font-weight: bold">#xxxx：</a>
      &nbsp;
      <a href="https://h5mota.com/user/user.php?uid=2082" style="color:red;text-decoration:none;" target="_blank">
      <b>谁谁谁</b>
      </a> 
      最后编辑于 
      <span style="color:gray" title="发表于 2019-12-02 22:51:30">2019-12-02 23:23:08</span>
      <copyButton :value='allContent'></copyButton>
    </p>
  </div>
  <show :word='allContent' class="show"></show>
  <hr>
</div>
</template>

<script>
import show from './components/show.vue'
import inputText from './components/input-text'
import inputHelp from './components/input-help'
import inputAwesome from './components/input-awesome'
import copyButton from './components/copyButton'
export default {
  name: 'myApp',
  data() {
    return {
      content:'',
      tempContent:'',
      tabs:['输入栏','特效字体输入','输入帮助'],
      coms:['inputText','inputAwesome','inputHelp'],
      currentIndex:0,
      //debounce 效果 ， 临时值debounce 才触发 组件渲染值的改变 进而渲染
      timer:-1,
      allContent:'',
    }
  },
  components:{
    show,
    inputText,
    inputHelp,
    inputAwesome,
    copyButton,
  },
  computed: {
    currentTab:function(){
      return this.coms[this.currentIndex];
    },
    timerContent:function(){
      return this.content+this.tempContent;
    },
  },
  watch:{
    timerContent:function(){
      this.debounce();
    }
  },
  methods: {
    //缓冲
    debounce(){
      clearTimeout(this.timer);
      this.timer=setTimeout(()=>{
        this.allContent=this.timerContent;
      },150);
    },
    
  },
}
</script>

<style lang="css">
.content {
  height: 300px;
  margin: 10% 10%;
}
.show {
  width: 100%;
}
.tab {
  position: relative;
  overflow: hidden;
  width: 90%;
  margin: auto;
  margin-top: 10px;
}
.tabLeft {
  border-top-left-radius: 15px;
  border-bottom-left-radius: 15px;
}
.tabRight {
  border-top-right-radius: 15px;
  border-bottom-right-radius: 15px;
}
.tabBar {
  width: 450px;
  margin: auto;
}
.tab-button{
  position: relative;
  font-size: 18px;
  border: 1px solid transparent;
  background-color: rgba(128, 128, 128, 0.479);
  background-clip: border-box;
  width: 30%;
  border-style: dotted none none none;
  border-width: 2px;
  padding-bottom: 5px;
}
.tab-button:hover{
  color: tomato;
}
.active {
  border-style: dotted none none none;
  border-color: #137333;
  background-color: rgba(255, 255, 255, 0.178);
  color: tomato
}
.change-enter-active, .change-leave-active {
  transition: opacity .3s ease;
}
.change-leave-to,.change-enter
/* .component-fade-leave-active for below version 2.1.8 */ {
  opacity: 0;
}
.change-enter-to,.change-leave {
  opacity: 1;
}
hr {
  width:100%;
  margin:15px 0;
  border: 0;
  height: 2px;
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0));
}
button {
  margin: 0;
padding: 0;
border: 1px solid transparent; 
outline: none;  
}
textarea {
  border: 1px dotted transparent;
  border-radius: 5px;
  background-color: rgba(241,241,241,.98);
  outline: none;
}
textarea:active {
  border-color: #137333;
}
textarea:focus {
  border-color: #137333;
}
</style>