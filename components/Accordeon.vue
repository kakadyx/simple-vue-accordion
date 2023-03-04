<template>
<details  :class="{'transition-blocked': transitionBlocked}" :style="cssVars" class="dropdown" ref="dd">
  <summary @click.prevent="onSummaryClick" ref="header" class="summary"> Summary </summary>
  <div
    ref="content"
    class="content"
    @transitionrun="transitionBlocked = true"
    @animationstart="transitionBlocked = true"
    @transitionend="transitionBlocked = false"
    @animationend="transitionBlocked = false"
  >
   <p>Lorem ipsum dolor sit amet,
     consectetur adipisicing elit.
     Architecto asperiores debitis
     dolor enim et fugiat, incidunt
     ipsum laboriosam maiores minima
     nulla numquam obcaecati ratione
     tempore totam unde voluptate.
     Rem, vitae?</p>
    <slot name="content"></slot>
  </div>
</details>
</template>

<script>
export default {
  name: 'Accordeon',
  mounted(){
    this.headerHeight = this.$refs.header.offsetHeight
    this.borderWidth = parseInt(getComputedStyle(this.$refs.dd,null).getPropertyValue('border-width'), 10)
    this.fullHeight = this.headerHeight + this.borderWidth * 2
    this.resizeObserver = new ResizeObserver(entries => {
      entries.forEach(({ target }) => {
        this.contentHeight = target.offsetHeight
        this.calculateFullHeight()
      })
    })


  },
  data(){
    return {
      headerHeight: null,
      contentHeight: null,
      fullHeight: null,
      transitionTime: 300,
      borderWidth: null,
      resizeObserver: null,
      once: false,
      transitionBlocked: false
    }
  },
  computed:{
    cssVars(){
      return `
        --height: ${this.fullHeight}px;
        --transition-time: ${this.transitionTime}ms;
      `
    }
  },
  methods:{
    calculateFullHeight(){
      this.fullHeight = this.headerHeight + this.contentHeight + this.borderWidth * 2
    },
    onSummaryClick(){
      const isOpen = this.$refs.dd.hasAttribute('open')
      this.toggle(isOpen)
    },
    toggle(isOpen){
      if (isOpen) {
        this.contentHeight = null
        this.__timeoutId = setTimeout(() => {
          this.$refs.dd.removeAttribute('open')
        }, this.transitionTime)
      } else {
        this.__timeoutId && clearTimeout(this.__timeoutId)
        this.contentHeight = this.$refs.content.offsetHeight
        this.$refs.dd.setAttribute('open', '')
        if(!this.once)
        setTimeout(() => {
          this.resizeObserver.observe(this.$refs.content)
        }, this.transitionTime)
      }
      this.calculateFullHeight()
    }
  },
}
</script>

<style lang="scss" scoped>
.transition-blocked{
  transition: none !important;
}
.dropdown{
  --height: 999px;
  --transition-time: 300ms;
  border: 1px solid crimson;
  border-radius: 10px;

  max-height: var(--height);
  transition: var(--transition-time) all;
  overflow: hidden;
}
.summary{
  border: 4px solid blueviolet;
  border-radius: 9px;
  padding: 10px;
  user-select: none;
  cursor: pointer;
}
.content{
  border: 4px solid greenyellow;
  border-radius: 9px;
  padding: 10px;
}
</style>
