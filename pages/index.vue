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
import scrolldatepicker from "@/components/Scrolldatepicker";
export default {
  name: 'form-data',
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

<style lang="scss">
  @import "@/assets/scrolldatepicker.scss";
</style>