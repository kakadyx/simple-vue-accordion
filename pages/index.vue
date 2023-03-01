<template>
  <div class="__main">
      <Accordeon class="accordeon">
        <template  #content >
          <Accordeon class="accordeon">
            <template  #content >
              <Accordeon class="accordeon">
                <template  #content >
                  <Accordeon class="accordeon">

                  </Accordeon>
                </template>
              </Accordeon>
            </template>
          </Accordeon>
        </template>

      </Accordeon>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage'
})
</script>

<style lang="scss" scoped>
.__main{
  width: 100vw;
  padding: 50px;
  min-height: 100vh;
  .accordeon {
    max-width: 50%;
  }
}

</style>
<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
