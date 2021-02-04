# Vue Nuxt Scroll Datepicker - Cashbac


![test](https://img.shields.io/npm/dt/vue-scroll-datepicker-cashbac.svg?style=flat-square)
![test](https://img.shields.io/npm/v/vue-scroll-datepicker-cashbac/latest.svg?style=flat-square)
![test](https://img.shields.io/badge/code_style-standard-brightgreen.svg?style=flat-square)

In this article, we’ll look at how date and time picker and a virtual scroll.
Fast, powerful and easy to use component datetime picker for Nuxt Vue.

## Demo

See demo [here](https://miichlas.online/demo/vue-scroll-datepicker-cashbac)

![vue-datepicker-picker-cashbac](https://res.cloudinary.com/cashbac-media/image/upload/v1612201376/ezgif.com-gif-maker_unxzkw.gif)

## Requirements

- Vue CLI `^@vue/cli 4.5.11`
- Vue Nuxt `v2.14.12`


## Usage

Then we import the styles from the package in the component.
v-model lets us save the selection to the datetime state.
It has many slots to let us customize any section of the date-time picker.


```javascript
npm install vue-scroll-datepicker-cashbac --save
```

```html
<template>
  <div class="container">
    <div>
      <h1>
        Scroll Datepicter
      </h1>
      <scrolldatepicker 
        placeholder="May 28, 1989"
        v-model="dateBirth"
        :theme-color="'#CDCDCD'"
        :min="'1970-01-01'"
        :max="'2010-01-01'"
        :input-class="'vue-date-input'"
        :input-style="{color:'#333333'}"
      />
    </div>
  </div>
</template>

<script>
import scrolldatepicker from 'node_modules/vue-scroll-datepicker-cashbac/components/Scrolldatepicker';
export default {
  data() {
    return {
      dateBirth: '',
      dataGender: 'Female',
      dataName: ''
    };
  },
  components: {
    scrolldatepicker
  }
}
</script>

<style lang="scss">
  @import "node_modules/vue-scroll-datepicker-cashbac/assets/scrolldatepicker.scss";
</style>
```

### Props:

| Name                  | Type                            | Default             | Description                                                                                                            |
| --------------------- | ------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| v-model, value        | String, Date, DateTime (luxon)  |      yyyy-LL-dd HH:mm:ss               | Value                                                                                                                  |
| value          | String                          | null | Value                         |
| max              | String, Date, DateTime (luxon)  | yyyy-LL-dd HH:mm:ss                | Max date                                                                                                               |
| min              | String, Date, DateTime (luxon)  | yyyy-LL-dd HH:mm:ss                | Min date                                                                                          
| placeholder          | String                          | null | 
| inputClass          | String                          | null | 
| inputStyle          | String                          | null | 
| themeColor          | String                          | null |

### Events:

| Name                  |
| --------------------- |
| close                 |
| open                  |
| change-month          |
| change-year           |
| change-decade         |

### Methods:

| Name                  | Description           |
| --------------------- | --------------------- |
| open                  | Open datetime picker  |
| close                 | Close datetime picker |

## Development

```bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate
```
