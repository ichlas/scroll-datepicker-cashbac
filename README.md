# Vue Nuxt Scroll Datepicker - Cashbac

In this article, we’ll look at how date and time picker and a virtual scroll.
Fast, powerful and easy to use component datetime picker for Nuxt Vue.

## Demo

See demo [here](https://miichlas.online/demo/vue-scroll-datepicker-cashbac)

![vue-datepicker-picker-cashbac](https://res.cloudinary.com/cashbac-media/image/upload/v1612190299/ezgif.com-gif-maker_bgnprm.gif)

## Requirements

- Vue CLI `^@vue/cli 4.5.11`
- Vue Nuxt `v2.14.12`


## NPMJS

[vue-scroll-datepicker-cashbac](https://www.npmjs.com/package/vue-scroll-datepicker-cashbac)

## Usage

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
import { mapMutations, mapGetters } from 'vuex'
import scrolldatepicker from 'vue-scroll-datepicker-cashbac';
export default {
  data() {
    return {
      dateBirth: '',
      dataGender: 'Female',
      dataName: '',
      lsForm: {
          strFullName: '',
          strBirthDate:'',
          strGender:''
      }
    };
  },
  components: {
    scrolldatepicker
  }
}
</script>
```

```scss
@import "@/assets/scrolldatepicker.scss"
```

### Props:

| Name                  | Required | Type                            | Default             | Description                                                                                                            |
| --------------------- | -------- | ------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| v-model, value        | *        | String, Date, DateTime (luxon)  |      yyyy-LL-dd HH:mm:ss               | Value                                                                                                                  |
| value          |          | String                          | null | Value                         |
| max              |          | String, Date, DateTime (luxon)  | yyyy-LL-dd HH:mm:ss                | Max date                                                                                                               |
| min              |          | String, Date, DateTime (luxon)  | yyyy-LL-dd HH:mm:ss                | Min date                                                                                          
| placeholder          |          | String                          | null | 
| inputClass          |          | String                          | null | 
| inputStyle          |          | String                          | null | 
| themeColor          |          | String                          | null |

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
