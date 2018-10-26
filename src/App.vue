<template>
  <div id="app" class='bg-grey'>
    <router-view/>
  </div>
</template>

<script>


export default {
  name: 'App',
};
</script>


<style lang="less">
#app {
  height: 100vh
}
.container {
  width: 1200px;
  margin: 0 auto;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: inherit;
  -ms-flex-align: inherit;
  align-items: inherit;
  -webkit-box-orient: inherit;
  -webkit-box-direction: inherit;
  -ms-flex-direction: inherit;
  flex-direction: inherit;
  }
@media only screen and (max-width: 768px) {
  body, html {
    overflow-x: hidden;
    overflow-y: hidden;
  }
  .container{
    width: 100%!important;
    margin-left: 0!important;
    margin-right: 0!important;
  }
  .left-segment.links, .right-segment.links {
    display: none!important
  }
}
@media only screen and (min-width: 768px) {
  .container {
    width: 723px;
    margin-left: auto!important;
    margin-right: auto!important;
  }
}
@media only screen and (min-width: 1280px) {
  .container {
    width: 1127px;
    margin-left: auto!important;
    margin-right: auto!important;
  }
}
.grid {
  margin-top: -1rem;
  margin-bottom: -1rem;
  margin-left: -1rem;
  margin-right: -1rem;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
  -ms-flex-direction: row;
  flex-direction: row;
  -ms-flex-wrap: wrap;
  flex-wrap: wrap;
  -webkit-box-align: stretch;
  -ms-flex-align: stretch;
  align-items: stretch;
  padding: 0;
}
.column {
  width: 33.33333333%!important;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-direction: column;
  flex-direction: column;
  vertical-align: middle;
  -ms-flex-item-align: center!important;
  align-self: center!important;
  padding-top: 1rem;
  padding-bottom: 1rem;
  position: relative;
  display: inline-block;
  width: 6.25%;
  padding-left: 1rem;
  padding-right: 1rem;
  vertical-align: top;
}
.center {
  text-align: center
}
.right {
  text-align: right;
}
.left {
  text-align: left
}
.move-up {
  margin-top: -0.1em
}
.bg-grey {
  background: rgb(236, 239, 241)!important;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
body {
  margin: 0px
}
</style>
