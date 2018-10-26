<template>
<div class="navi">
  <div class='header'>
    <div class="container navcont">
      <div class='left-segment links'>
        <router-link to='/'> <div class="item">Item1</div></router-link>
        <router-link to='/'> <div class="item">Item2</div></router-link>
        <router-link to='/'> <div class="item">Item3</div></router-link>
        <router-link to='/'> <div class="item">Item4</div></router-link>
      </div>
      <div class='left-segment phone'>
        icon
      </div>
      <div class='brand-icon item'>
        <router-link to='/'><img src="../assets/site-logo.svg" class='brand-image' alt="logo"> </router-link>
      </div>
      <div class='right-segment links'>
        <div class="item">Item1</div>
        <div class="item">Item2</div>
        <div class="item">Item3</div>
        <div class="item">Item4</div>
      </div>
      <div class='right-segment phone'>
        icon
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'header'
};
</script>

<style lang='less' scoped>

.navcont {
  -webkit-box-pack: justify;
  justify-content: space-between;
}
.navi {
    z-index: 999;
    position: relative;
    background: #fff;
    -webkit-box-shadow: 0 1px 2px 0 rgba(34,36,38,.15);
    box-shadow: 0 1px 2px 0 rgba(34,36,38,.15);
    margin: 1rem 0;
    padding: 1em 1em;
    border-radius: .28571429rem;
    border: 1px solid rgba(34,36,38,.15);
}
.header {
  position: fixed;
  top: 0;
  left: 0;
  max-width: 100vw;
  width:100%;
  background: #fff;
  font-weight: 400;
  display: flex;
  display: -webkit-box;
  height: 2.86rem;
  border: 1px solid rgba(34,36,38,.15);
    -webkit-box-shadow: 0 1px 2px 0 rgba(34,36,38,.15);
    box-shadow: 0 1px 2px 0 rgba(34,36,38,.15);
  > * > *, > * > * > * {
    display: flex;
  }
}
.links a {
  text-decoration: none;
}
.brand-image {
  width: 80px;
  height: auto;
  display: block;
  position: relative;
  display: inline-block;
  max-width: 100%;
  background-color: transparent;
}
*, :before {
  -webkit-box-sizing: inherit;
  box-sizing: inherit;
}
.item {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  position: relative;
  vertical-align: middle;
  line-height: 1;
  text-decoration: none;
  -webkit-tap-highlight-color: transparent;
  -webkit-box-flex: 0;
  -ms-flex: 0 0 auto;
  flex: 0 0 auto;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  background: 0 0;
  padding: 0.93em 1.25em;
  text-transform: none;
  color: rgba(0, 0, 0, 0.87);
  font-weight: 400;
  -webkit-transition: background 0.1s ease, color 0.1s ease,
    -webkit-box-shadow 0.1s ease;
  transition: background 0.1s ease, color 0.1s ease,
    -webkit-box-shadow 0.1s ease;
  transition: background 0.1s ease, box-shadow 0.1s ease, color 0.1s ease;
  transition: background 0.1s ease, box-shadow 0.1s ease, color 0.1s ease,
    -webkit-box-shadow 0.1s ease;
  &:hover {
    cursor: pointer;
    background: rgba(0, 0, 0, 0.03);
    color: rgba(0, 0, 0, 0.95);
  }
}
.phone {
  display:none
}

@media only screen and (max-width: 768px) {
  .phone {
    display:block;
    padding:10px
  }
}
</style>
