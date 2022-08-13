<template>
  <input class="hueta" v-for="(item,index) in 5" v-bind:key="item.id" v-model="codeArray[index]" @paste="onPaste" maxlength="1">
  <br>
  <br>
  <button id="btn" :disabled="isDisabled">Submit</button>

  <p>Введённое сообщение: {{ codeArray }}</p>

</template>
<script>

export default {
  name: 'HelloWorld',
  data() {
    return {
      codeArray: [],
      txts: '',
      wholeCode: '',
      isDisabled: true
    }
  },
  methods: {
    onPaste(evt) {
      let pasted = evt.clipboardData.getData('text/plain')
      // if (pasted.length === 5 && )
      if (parseInt(pasted).toString().length === 5){
        for (let i = 0; i < 5; i++) {
          this.codeArray[i] = pasted[i]
        }
        this.txts[4].focus()
        this.wholeCode = pasted
      }else {
        evt.preventDefault()
      }
      if (this.wholeCode){
        this.isDisabled = false
      }
    },

    test(el) {
      let txts = document.querySelectorAll(el);// Получаем все входные объекты
      let arr = this.codeArray

      for (let i = 0; i < txts.length; i++) {
        let t = txts[i];
        t.onkeydown = function (e) {
          let e2 = e ? e : event;
          let k = e2.keyCode || e2.which;
          if (k === 8) {
            if ((arr[i] === '' || arr[i] === undefined) && i > 0){
              txts[i-1].focus()
            }
          }
        }

        t.onkeyup = function (e) {
          let e2 = e ? e : event;
          let keyCode = e2.keyCode || e2.which;

          if (keyCode !== 8) { // Если не удалять событие
            if (keyCode < 48 || keyCode > 57) {
                arr[i] = parseInt(arr[i]) ? arr[i] : ''
            } else if (i<4){
              txts[i+1].focus()
            }
          }
        }
      }
    }
  },
  mounted() {
    this.txts = document.querySelectorAll('.hueta')
    this.test('.hueta');
  },
  watch: {
    codeArray: {
      handler(newValue) {
        this.isDisabled = newValue.join('').length !==5
      },
      deep: true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
