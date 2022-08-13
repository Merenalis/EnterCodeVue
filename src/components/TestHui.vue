<template>
  <div class="container">
    <div class="modal-backdrop"  v-show="isModalVisible"    @click="closeBackground"  >
      <div class="modal" @click.stop>
        <div class="code-input-main" >
          <input
              pattern="\d*"
              class="code-input-main-item"
              id="phoneCode1"
              v-model="phoneCode0"
              oninput="value=value.length>1?value.substr(0,1):value;"
              maxlength="1"
              type="number"/><input
            pattern="\d*"
            class="code-input-main-item"
            id="phoneCode2"
            v-model="phoneCode1"
            oninput="value=value.length>1?value.substr(0,1):value;"
            maxlength="1"
            type="number"/><input
            pattern="\d*"
            class="code-input-main-item"
            id="phoneCode3"
            v-model="phoneCode2"
            oninput="value=value.length>1?value.substr(0,1):value;"
            maxlength="1"
            type="number"/><input
            pattern="\d*"
            class="code-input-main-item"
            id="phoneCode4"
            v-model="phoneCode3"
            oninput="value=value.length>1?value.substr(0,1):value;"
            maxlength="1"
            type="number"/><input
            pattern="\d*"
            class="code-input-main-item"
            id="phoneCode5"
            v-model="phoneCode4"
            oninput="value=value.length>1?value.substr(0,1):value;"
            maxlength="1"
            type="number"/><input
            pattern="\d*"
            class="code-input-main-item"
            id="phoneCode6"
            v-model="phoneCode5"
            oninput="value=value.length>1?value.substr(0,1):value;"
            maxlength="1"
            type="number"/>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'TestHui',
  data() {
    return {
      isModalVisible:true, // Отображать ли всплывающее окно для получения кода подтверждения
      phoneCode:"",  // Проверочный код 6 цифр
      phoneCode0:"", // Значение поля ввода 1
      phoneCode1:"", // Значение поля ввода 2
      phoneCode2:"", // Вводим значение поля 3
      phoneCode3:"", // Вводим значение поля 4
      phoneCode4:"", // Вводим значение поля 5
      phoneCode5:"", // Вводим значение поля 6
      telephone:"1333***8912", // телефонный номер

    };
  },
  mounted() {
    this.goNextInput('.code-input-main-item');// Реализуем автоматический фокус поля ввода
  },
  methods: {
    closeBackground(){ // Закрываем всплывающее окно с кодом подтверждения
      this.isModalVisible=false
    },
    goNextInput(el){ // Добиваемся автоматического фокуса поля ввода
      let that = this;
      let txts = document.querySelectorAll(el);// Получаем все входные объекты
      for(let i = 0; i<txts.length;i++){ // Циклический ввод для добавления событий мониторинга
        let t = txts[i];
        t.index = i; // вводим значение индекса присваивания
        t.onkeydown=function(e){ // когда клавиатура нажата
          let e2 = e ? e : event;
          let k = e2.keyCode || e2.which;
          if (k === 8) { // Если это событие удаления
            if(this.value!==""){ // Если текущий ввод не пустой
              this.value = ""; // Очистить текущее входное значение
              switch(this.index) { // Определяем, какое входное значение необходимо очистить в соответствии со значением индекса
                case 0:
                  that.phoneCode0 ="";
                  break;
                case 1:
                  that.phoneCode1 ="";
                  break;
                case 2:
                  that.phoneCode2 ="";
                  break;
                case 3:
                  that.phoneCode3 ="";
                  break;
                case 4:
                  that.phoneCode4 ="";
                  break;
                case 5:
                  that.phoneCode5 ="";
                  break;
              }
            }else{ // Если текущий ввод пуст, фокус на предыдущем вводе
              let back = this.index-1;
              that.value = "";
              if(back <0) return; // Если значение индекса меньше 0, возвращаем
              txts[back].focus();
            }
          }
        }
        t.onkeyup=function(e){ // когда клавиатура отпущена
          let e2 = e ? e : event;
          let k = e2.keyCode || e2.which;
          if (k !== 8) { // Если не удалять событие
            that.value=this.value.replace(/^(.).*$/,'$1');// Текущее поле ввода сохраняет цифру
            let next = this.index + 1; // значение индекса плюс 1
            if(next > txts.length-1 ) // Если это последний вход, событие выполняется
            {   that.phoneCode = "";
              that.phoneCode = that.phoneCode0+that.phoneCode1+that.phoneCode2+that.phoneCode3+that.phoneCode4+that.phoneCode5;
              // that.CheckSmsCode (); // проверяем правильность кода подтверждения
              return;
            }
            if (this.value) { txts[next].focus(); } // Если есть значение, переход к следующему вводу
          }
        }
      }
    },

  } // methods
};
</script>
<style scoped >

.modal-backdrop {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}
/* Всплывающие окна */
.modal {
  width:670px;
  height:480px;
  background:rgba(255,255,255,1);
  border:2px solid rgba(230,230,230,1);
  border-radius:10px;
  padding: 0 92px;
  /* Ряд */
}
.font1{
  width:210px;height:40px;font-size:40px;font-family:Microsoft YaHei;font-weight:400;
  color:rgba(40,40,40,1); line-height:40px;margin-top: 58px;
}
.font2{
  width:550px;height:29px;font-size:28px;font-family:Microsoft YaHei;font-weight:400;color:rgba(40,40,40,1);line-height:29px;
  margin-top: 40px;
}
.code-input-main {
  width:100%;
  height: 60px;
  text-align: center;
  margin-top: 56px;
}
.code-input-main-item {
  width:60px;
  height:60px;
  padding: 2px;
  border:2px solid rgba(230,230,230,1);
  border-radius:6px;
  display: inline-block;

}
input {
  background: none;
  outline: none !important;
  -webkit-appearance: none !important;
  -webkit-appearance: none !important;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0) !important;
}
</style>
