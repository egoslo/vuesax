<template lang="html">
  <transition name="popup-t">
    <div
      v-show="active"
      ref="con"
      :class="[`vs-popup-${color}`,{'fullscreen':fullscreen}]"
      class="vs-component con-vs-popup"
      @click="close($event,true)">
      <div
        :style="styleCon"
        class="vs-popup-dark"/>
      <div
        ref="popupx"
        :style="stylePopup"
        class="vs-popup">

        <!-- //header -->
        <header :style="styleHeader">
          <div class="con-title-after">
            <h3>{{ title }}</h3>
          </div>
          <span
            v-if="!buttonCloseHidden"
            translate="no"
            class="vs-popup-cancel material-icons notranslate"
            @click="close">close</span>
        </header>

        <!-- // slots  -->
        <div
          :style="styleContent"
          :class="classContent"
          class="vs-popup-text">
          <slot/>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import _color from '../../utils/color.js'
export default {
  name:'VsPopup',
  props:{
    color:{
      default:'primary',
      type:String
    },
    active:{
      default:false,
      type: Boolean
    },
    title:{
      default:'popup',
      type:String
    },
    buttonCloseHidden:{
      default:false,
      type:Boolean
    },
    fullscreen:{
      default:false,
      type:Boolean
    },
    backgroundColor:{
      default:null,
      type:String
    },
    backgroundColorPopup:{
      default:'rgb(255,255,255)',
      type:String
    },
    styleContent:{
      default:null,
      type: String
    },
    classContent:{
      default:null,
      type: String
    }
  },
  computed:{
    styleHeader(){
      return {
        color: _color.getColor(this.color,1),
      }
    },
    styleAfter(){
      return {
        background: _color.getColor(this.color,1)
      }
    },
    styleCon(){
      return {
        background: _color.getColor(this.backgroundColor,1)
      }
    },
    stylePopup(){
      return {
        background: _color.getColor(this.backgroundColorPopup,1)
      }
    }
  },
  mounted(){
    this.insertBody()
  },
  methods:{
    giveColor(color){
      return _color.rColor(color)
    },
    close(event,con){
      if(con){
        if(event.target.className.indexOf('vs-popup-dark')!=-1){
          this.$emit('update:active',false)
        }
      } else {
        if(event?event.target.className.indexOf('vs-popup-cancel')!=-1:false ){
          this.$emit('update:active',false)
        }
      }
    },
    insertBody(){
      let elx = this.$refs.con
      document.body.insertBefore(elx, document.body.firstChild)
    },
  }
}
</script>
