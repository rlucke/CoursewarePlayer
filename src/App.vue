<template>
  <div id="cwp">
    <Courseware />
  </div>
</template>

<script>
import Courseware from './components/Courseware'

export default {
  name: 'cwp',
  components: {
    Courseware
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Lato&display=swap');
#cwp {
  font-family: Lato,sans-serif;
  font-size: 14px;
}
h2, h3 {
  font-size: 16px;
}
h1, h2, h3, h4, h5, h6 {
    margin-bottom: 10px;
}
h1, h2, h3 {
    margin-top: 20px;
}
h1, h2, h3, h4, h5, h6 {
    font-family: inherit;
    font-weight: 600;
    line-height: 1.1;
    color: 
    #444;
}
button::-moz-focus-inner {
  border: 0;
}
</style>
