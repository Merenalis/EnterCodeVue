<template>
  <input class="code-cell" v-for="(item,index) in 5" v-bind:key="item.id" v-model="codeArray[index]" @paste="onPaste" maxlength="1">
  <br>
  <br>
  <button id="btn" :disabled="isDisabled">Submit</button>

  <p>Введённое сообщение: {{ codeArray }}</p>

</template>
<script>

export default {
  name: 'CodeCells',
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

    main(el) {
      let txts = document.querySelectorAll(el);
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

          if (keyCode !== 8) {
              if (parseInt(arr[i])){
                arr[i] = parseInt(arr[i])
                txts[i+1].focus()
              } else {
                arr[i] =''

              }
          }
        }
      }
    }
  },
  mounted() {
    this.txts = document.querySelectorAll('.code-cell')
    this.main('.code-cell');
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
