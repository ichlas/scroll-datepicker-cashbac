<template>
  <div>
    <span class="arrow" :class="{'active': popShow}"></span>
    <input type="text" class="vue-component" v-model="selectNilai" @focus="showPanel" :placeholder="placeholder">
    <input type="hidden" readonly="readonly" :class="inputClass" :style="inputStyle" :placeholder="placeholder" v-model="selectValue" @focus="showPanel"/>
    <div class="vue-date-select-container" :class="{'vm-date-show': popShow}"
        @mousemove="handleTouch($event)"
        @mouseup="handleTouch($event)"
        @touchend="clearHover"
    >
      <div class="vm-dialog-content s" :style="{'margin-top': parseInt((windowHeight-260)*0.4)+'px'}">
        <div class="vm-wheels">
          <div class="vm-wheel" v-for="(item, wheelIndex) in wheels"
            @touchstart="handleTouch($event, wheelIndex)"
            @touchmove="handleTouch($event, wheelIndex)"
            @touchend="handleTouch($event, wheelIndex)"
            @mousedown="handleTouch($event, wheelIndex)"
            @mousewheel="handleTouch($event, wheelIndex)"
            @DOMMouseScroll="handleTouch($event, wheelIndex)">
            <div class="vm-line" :style="{borderColor:themeColor}"></div>
            <div class="vm-items-wrapper" :class="{'anim': item.anim}"
            :style="{'transform':'translate3d(0,'+ item.translateY +'px, 0)', 'transition-duration': item.anim ? item.transitionTime : '0s' }">
              <div v-for="(optionItem, itemIndex) in item.data"
                class="vm-option"
                :class="wheelIndex == 1 ? 'bulan' : 'normal'"
                @click="handleSingleClick($event, wheelIndex, itemIndex)"
                @touchstart="hoverClass($event, wheelIndex, itemIndex)"
                @mousedown="hoverClass($event, wheelIndex, itemIndex)">
                <span v-if="wheelIndex != 1">
                  {{ (optionItem < 10 ? '0'+optionItem : optionItem) }}
                </span>
                <span v-else>
                  {{ callBulan(wheelIndex, optionItem) }}
                </span>
              </div>
            </div>
          </div>
        </div>
        <div class="vm-btns">
          <div class="vm-btn" @click="getSelectData" 
            @touchstart="hoverClass($event, -2, -2)"
            @mousedown="hoverClass($event, -2, -2)">
            Choose
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "vuedatereward",
  props: {
    value: {
      type: String,
      default: ''
    },
    min: {
      type: String,
      default: ''
    },
    max: {
      type: String,
      default: ''
    },
    placeholder: {
      type: String,
      default: ''
    },
    inputClass: {
      type: [String, Object, Array],
      default: ''
    },
    inputStyle: {
      type: [String, Object, Array],
      default: ''
    },
    themeColor: {
      type: String,
      default: '#03a9f4'
    }
  },
  data: function () {
    return {
      popShow: false,
      selectValue: '',
      selectNilai: '',
      wheels :[
        {
          type: 'month',
          translateY: 0,
          anim : false,
          transitionTime: '700ms',
          data: []
        },
        {
          type: 'day',
          translateY: 0,
          anim : false,
          transitionTime: '700ms',
          data: []
        },
        {
          type: 'year',
          translateY: 0,
          anim : false,
          transitionTime: '700ms',
          data: []
        }
      ],
      startY: 0,
      moveY: 0,
      oldMoveY: 0,
      moveEndY: 0,
      offsetDistance: 0,
      offset: 0,
      oversizeBorder: 0,
      startTime: 0,
      liHeight : 40,
      minDate: '',
      maxDate: '',
      initDate: '',
      clickFlag: false,
      activeClick: {
        wheelIndex: -100,
        itemIndex: -100
      },
      activeWheelIndex: 0,
      windowHeight: 300
    }
  },
  computed: {
    hoverColor: function () {
      var hex = this.themeColor,
        opacity = 0.08,
        result = '';
      if(hex.replace(/\s+/g, '').length === 7){
        result = 'rgba(' + parseInt('0x' + hex.slice(1, 3)) + ',' +
                parseInt('0x' + hex.slice(3, 5)) + ',' +
                parseInt('0x' + hex.slice(5, 7)) + ',' + opacity + ')';
      }else{
        var rgb = hex.split('(')[1].split(')')[0].split(',');
        result = 'rgba(' + rgb[0].trim() + ',' + rgb[1].trim() + ',' + rgb[2].trim() + ',' + opacity + ')';
      }
      return result||'';
    }
  },
  mounted: function(){
    this.initSetting();
    this.initOption();
    this.initListener();
  },
  methods:{
    initListener: function(){
        var _this = this;
        window.addEventListener("resize", function(){
            _this.windowHeight = 300;
        },false);
    },
    initSetting: function(){
      this.initDate =  this.value&&this.checkIsFormatStr(this.value)? this.value : this.getDateStr(new Date());
      this.maxDate = this.max && this.checkIsFormatStr(this.max)? this.max : this.getDateStr(new Date(), 2);
      this.minDate = this.min && this.checkIsFormatStr(this.min)? this.min : this.getDateStr(new Date(), -10);
    },
    initOption: function(){
      var maxDateObj = this.getDateStrObj(this.maxDate);
      var minDateObj = this.getDateStrObj(this.minDate);
      var initDateObj = this.getDateStrObj(this.initDate);

      for(var i=minDateObj.year; i<=maxDateObj.year; i++){
        this.wheels[0].data.push(i);
      }
      for(var j=1; j<=12; j++){
        this.wheels[1].data.push(j);
      }
      for(var k=1; k<=this.calcDays(initDateObj.year, initDateObj.month); k++){
        this.wheels[2].data.push(k);
      }
      this.locateWheelByVal(this.initDate);
    },
    locateWheelByVal: function(dateString){
      var minDateObj = this.getDateStrObj(this.minDate);
      // console.log(1)
      var dateObj = this.getDateStrObj(dateString);
      this.wheels[0].translateY = this.getDistanceByIndex(dateObj.year-minDateObj.year);
      this.wheels[1].translateY = this.getDistanceByIndex(dateObj.month-1);
      this.wheels[2].translateY = this.getDistanceByIndex(dateObj.day-1);
    },
    descBulan: function(blnNum){
      let returns = '';
      if(blnNum == '01' || blnNum == 1){
        returns = 'January'
      }else if(blnNum == '02' || blnNum == 2){
        returns = 'February'
      }else if(blnNum == '03' || blnNum == 3){
        returns = 'March'
      }else if(blnNum == '04' || blnNum == 4){
        returns = 'April'
      }else if(blnNum == '05' || blnNum == 5){
        returns = 'May'
      }else if(blnNum == '06' || blnNum == 6){
        returns = 'June'
      }else if(blnNum == '07' || blnNum == 7){
        returns = 'July'
      }else if(blnNum == '08' || blnNum == 8){
        returns = 'August'
      }else if(blnNum == '09' || blnNum == 9){
        returns = 'September'
      }else if(blnNum == 10){
        returns = 'October'
      }else if(blnNum == 11){
        returns = 'November'
      }else if(blnNum == 12){
        returns = 'December'
      }
      return returns
    },
    callBulan: function(status, numbers){
      let returns = '';
      if(status == 1){
        returns = this.descBulan(numbers);
      }
      return returns
    },
    getDateStrObj: function(dateString, offsetYear, offsetMonth, offsetDay){
      var tempArr = dateString.split('-');
      return {
          year: ~~tempArr[0],
          month: ~~tempArr[1],
          day: ~~tempArr[2]
      }
    },
    addPrefix: function(num){
        return num < 10 ? '0' + num : num;
    },
    getDateStr: function(dateObj, offsetYear, offsetMonth, offsetDay){
        var tempArr = [];
        tempArr.push(dateObj.getFullYear()+(offsetYear||0));
        tempArr.push(this.addPrefix(dateObj.getMonth()+1+(offsetMonth||0)));
        tempArr.push(this.addPrefix(dateObj.getDate()+(offsetDay||0)));
        return tempArr.join('-'); 
    },
    checkIsFormatStr: function(dateString){
        if(dateString && typeof(dateString)=='string'){
            var tempArr = dateString.split('-');
            if(tempArr.length>2){
                var year = ~~tempArr[0],
                    month = ~~tempArr[1],
                    day = ~~tempArr[2];
                if((year>0&&year<10000)&&(month>=1&&month<=12)&&(day>=1&&day<=this.calcDays(year,month))){
                    return true;
                }
            }
        }
        // console.warn('');
        return false;
    },
    getDistanceByIndex: function(index){
        return (2-index)*this.liHeight;
    },
    getIndexByDistance: function(translateY){
        return parseInt(-translateY/this.liHeight)+2;
    },
    getWheelData: function(wheelIndex){
      var dataIndex = this.getIndexByDistance(this.wheels[wheelIndex].translateY);
      dataIndex = dataIndex<0 ? 0 : dataIndex; 
      dataIndex = dataIndex>=this.wheels[wheelIndex].data.length? this.wheels[wheelIndex].data.length-1 : dataIndex; 
      return this.wheels[wheelIndex].data[dataIndex];
    },
    calcDays: function(year, month) {
      return new Date(year, month, 0).getDate();
    },
    fixPosition: function(distance){
      return Math.round(distance/this.liHeight) * this.liHeight;
    },
    checkIsOverBorder: function(curWheelObj){
      var _this = this;
      this.oversizeBorder = -(curWheelObj.data.length-3)*this.liHeight;
      // console.log(1)
      if(curWheelObj.translateY > 2 * this.liHeight){
        setTimeout(function(){
            curWheelObj.transitionTime = '700ms';
            curWheelObj.translateY = 2 * _this.liHeight;
        }, 100);
      }else if(curWheelObj.translateY < this.oversizeBorder){
        setTimeout(function(){
            curWheelObj.transitionTime = '700ms';
            curWheelObj.translateY = _this.oversizeBorder;
        }, 100);
      }
    },
    updateDays: function(){
      var newMonthDaysNum = this.calcDays(this.getWheelData(0), this.getWheelData(1));
      if(newMonthDaysNum>0 && this.wheels[2].data.length != newMonthDaysNum){
        var tempArr = [];
        for(var k = 1; k <= newMonthDaysNum; k++){
            tempArr.push(k);
        }
        this.wheels[2].data = tempArr;
        this.checkIsOverBorder(this.wheels[2]);
      }
    },
    handleTouch: function(e, index) {
        e = e || window.event;
        var curWheelObj = typeof(index)=='number' ? this.wheels[index]: this.wheels[this.activeWheelIndex];
        switch (e.type){
            case 'touchstart':
            case 'mousedown':
                if(e.type == 'touchstart'){
                    this.startY = e.touches[0].clientY;
                }else{
                    this.startY = e.clientY;
                    this.activeWheelIndex = index;
                    this.clickFlag = true;
                    e.preventDefault();
                }
                curWheelObj.anim = false;
                this.oldMoveY = this.startY;
                this.startTime = (new Date()).getTime();
                break;

            case 'touchend':
            case 'mouseup':
                if(e.type == 'touchend'){
                    this.moveEndY = e.changedTouches[0].clientY;
                }else{
                    this.moveEndY = e.clientY;
                    this.clickFlag = false;
                }

                this.offsetDistance = this.moveEndY - this.startY;
                curWheelObj.anim = true;
                curWheelObj.translateY = this.fixPosition(curWheelObj.translateY+this.offset);
                var offsetTime =  (new Date()).getTime() - this.startTime;
                var scrollSpeed = this.offsetDistance/offsetTime;
                var tempTime = Math.abs(parseInt(scrollSpeed*1000));
                curWheelObj.transitionTime = '700ms';
                // console.log(this.selectValue)
                if(Math.abs(scrollSpeed)>0.3){
                    curWheelObj.transitionTime = tempTime > 700? (tempTime+'ms') : '700ms';
                    curWheelObj.translateY = this.fixPosition(curWheelObj.translateY + scrollSpeed * 250);
                }
                this.checkIsOverBorder(curWheelObj);
                this.clearHover();
                break;

            case 'mousemove':
            case 'touchmove':
                e.preventDefault();
                if(e.type=='mousemove' && !this.clickFlag){
                    return false;
                }
                // console.log('1')
                this.moveY = e.type=='touchmove'? e.touches[0].clientY : e.clientY;
                this.offset = this.moveY - this.oldMoveY;
                curWheelObj.translateY += this.offset;
                this.oldMoveY = this.moveY;
                break;

            case 'mousewheel':
                curWheelObj.anim = true;
                curWheelObj.translateY = this.fixPosition(curWheelObj.translateY+parseInt((e.wheelDelta||e.detail)*0.3));
                this.oversizeBorder = -(curWheelObj.data.length-3)*this.liHeight; 
                this.checkIsOverBorder(curWheelObj);
                break;
        }
        if(index <= 1){
            this.updateDays();
        }
    },
    handleSingleClick: function(e, wheelIndex, itemIndex){
        if(Math.abs(this.offsetDistance)<30){
            this.wheels[wheelIndex].translateY = this.getDistanceByIndex(itemIndex);
        }
    },
    hoverClass: function(e, wheelIndex, itemIndex){
        this.activeClick.wheelIndex = wheelIndex;
        this.activeClick.itemIndex = itemIndex;
    },
    clearHover: function(){
        this.activeClick.wheelIndex = -100;
        this.activeClick.itemIndex = -100;
    },
    getSelectData: function(){
        var _this = this;
        var tempArr = [];
        this.wheels.forEach(function(item, wheelIndex){
          console.log(_this.getWheelData(wheelIndex))
          // tempArr.push('May 28, 1989');
          tempArr.push(_this.addPrefix(_this.getWheelData(wheelIndex)));
        });
        this.selectValue = tempArr.join('-');
        var dates = this.selectValue.split('-');
        this.selectNilai = _this.descBulan(dates[1])+' '+dates[2]+', '+dates[0];
        // console.log(122)
        this.$emit('input', this.selectValue);
        this.hidePanel();
    },
    hidePanel: function(){
        this.clearHover();
        this.popShow = false;
        if(this.selectValue){
            this.locateWheelByVal(this.selectValue);
        }
    },
    showPanel: function(){
        this.clearHover();
        this.popShow = true;
    },
  }
}
</script>
