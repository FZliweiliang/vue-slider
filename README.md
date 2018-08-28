# vue-slider
## Usage

#### Use:

```html
<template>
  <div>
    <slider :value-fun="sliderVal" :min="0" :max="100"></slider>
  </div>
</template>
<script>

import slider from 'vue-slider.vue'

new Vue({
  el: '#app',
  components: {
    slider
  },
  methods:{
    sliderVal(min,max,total){
       
    }
  },
});
</script>
```
<br>


## Options

### Props
| Props       | Type          | Default  | Description  |
| ----------- |:--------------| ---------|--------------|
| max   | Number        | auto | 默认最大值 初始位置 |
| min  | Number        | auto   | 默认最小值初始位置 |


### Function
| Name        | Type           | Description                |
| ----------- |:---------------| ---------------------------|
| valueFun    | Return: value  | valueFun(最小值,最大值,合计) |

### screenshot
<img src="https://github.com/474782977/vue-slider/blob/master/screenshot/1.jpg" width="320px" style="display:inline;">

## License

[MIT](LICENSE)
