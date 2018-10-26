<template lang="pug">
  div.statistics
    div.laptop
      div.label
        span {{label}}
      div.value 
        span {{value}}
    div.mobile
      hr
      span {{label}}:
      span.flo {{value}}
</template>

<script>
export default {
  name: 'StatCell',
  props: ['label', 'value']
}
</script>

<style>
.mobile hr {
  border: none; 
  color: rgba(0, 0, 0, 0.15); 
  background-color: rgba(0, 0, 0, 0.15); 
  height: 1px;
  margin: 4px 0;
}
.label {
  font-size: 1em;
  font-weight: 700;
  color: white;
  text-transform: uppercase;
  text-align: center;
}
.value {
  font-size: 1.5rem;
  font-weight: 400;
  line-height: 1em;
  color: white;
  text-transform: uppercase;
  text-align: center;
}
.flo {
  float:right;
}
.mobile {
  display: none;
  color: white;
}
@media only screen and (max-width:768px) {
  .laptop {
    display: none
  }
  .mobile {
    display: block
  }
  .data {
    padding: 0 20px;
  }
}
</style>
