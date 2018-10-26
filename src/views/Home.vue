<template>
  <div class="home">
    <HeaderEl class='block' />
    <MainInfo class='block' />
    <TableEl />
  </div>
</template>

<script>
import HeaderEl from '@/components/Header.vue';
import MainInfo from '@/components/MainInfo.vue';
import TableEl from '@/components/Table.vue';

export default {
  name: 'home',
  components: {
    MainInfo,
    HeaderEl,
    TableEl
  },
};
</script>

<style lang="less" scoped>
.block {
  position: relative;
  -webkit-box-shadow: none;
  box-shadow: none;
  margin: 0.3rem 0 0;
  padding: 1em 1em;
}
</style>
