<template>
  <div class="overlay">
    <div class="card">
      <h4>{{ usage }}</h4>
      <div class="editDate" v-if="type === 'editDate'">
        <div>
          <span>原來結單日期</span>
          <el-input :value="original" style="width: 95%" disabled />
        </div>
        <div>
          <span>更新結單日期</span>
          <el-input
            v-model="updated"
            style="width: 95%"
            placeholder="Please input"
          />
          <span id="alert" v-show="alertShow">請輸入新的結單日期</span>
        </div>
      </div>
      <div class="notify" v-else-if="type === 'notify'">
        <div>
          <span>顧客名單</span>
          <el-input
            :value="customerName"
            style="width: 95%"
            placeholder="Please input"
          />
        </div>
        <div>
          <span>訊息內容</span>
          <textarea
            v-model="notifyMessage"
            placeholder="請輸入通知訊息內容"
          ></textarea>
        </div>
      </div>
      <div class="addCus" v-else-if="type === 'addCus'">
        <div>
          <span>訂購數量</span>
          <el-input
            v-model="addNum"
            style="width: 95%"
            placeholder="Please input"
          />
        </div>
      </div>
      <div class="editDate" v-if="type === 'endOrder'">
        <div>
          <span>此次團購訂購商品數量</span>
          <el-input v-model="totalNum" style="width: 95%" placeholder="Please input"/>
        </div>
        <div>
          <span>此次訂購花費成本</span>
          <el-input
            v-model="cost"
            style="width: 95%"
            placeholder="Please input"
          />
        </div>
      </div>
      <div class="editDate" v-if="type === 'arriveManage'">
        <div>
          <span>到貨日期</span>
          <el-input :value="arriveDate" style="width: 95%" disabled/>
        </div>
        <div>
          <span>截止領取天數</span>
          <el-input
            v-model="dueDays"
            style="width: 95%"
            placeholder="Please input"
          />

        </div>
      </div>
      <div class="buttons">
        <store-button :action="'確認'" class="button" @click="check" />
        <button class="buttonCancel" @click="cancel">取消</button>
      </div>
    </div>
  </div>
</template>

<script>
  import { ref } from "vue";
  import StoreButton from "./StoreButton.vue";
  export default {
    props: ["usage", "original", "type", "customerName", ""],
    components: {
      StoreButton,
    },
    setup(props, { emit }) {
      const arriveDate = ref("2024/05/20");
      const dueDays = ref();
      const cost = ref();
      const totalNum = ref();
      const updated = ref("");
      const alertShow = ref(false);
      const notifyMessage = ref("");
      const addNum = ref();
      const alert = () => {
        if (updated.value == "") {
          alertShow.value = true;
        } else {
          alertShow.value = false;
        }
      };
      const cancel = () => {
        emit("isCanceled", false);
      };
      const check = () => {
        alert();
        if (!alertShow.value) {
          emit("isChecked", false);
          if (props.type === "editDate") {
            console.log(props.type);
            emit("check", updated.value);
            updated.value = "";
          }
        } else {
          emit("check", false);
        }
      };
      return {
        updated,
        cancel,
        check,
        alertShow,
        notifyMessage,
        addNum,
        totalNum,
        cost,
        arriveDate,
        dueDays,
      };
    },
  };
</script>

<style scoped>
  .overlay {
    position: absolute;
    left: 0;
    top: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    position: fixed;
    background-color: rgba(0, 0, 0, 0.6);
    z-index: 5;
  }
  .card {
    display: flex;
    flex-direction: column;
    gap: 40px;
    /* margin: 0 auto; */
    padding: 32px;
    width: 70%;
    height: fit-content;
    background-color: white;
    border-radius: 12px;
  }
  h4 {
    font-weight: bold;
  }
  .buttons {
    display: flex;
    gap: 8px;
  }
  .button {
    width: 200px;
    height: 100%;
  }
  .buttonCancel {
    width: 200px;
    height: 100%;
    padding: 13px;
    border: 1px #d4d4d8 solid;
    border-radius: 10px;
    background-color: white;
  }
  #cancel {
    height: 100%;
    border-radius: 10px;
  }
  .editDate {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  .editDate > div {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  #alert {
    /* font-size: 10px; */
    color: #dc2626;
  }
  /* ::v-deep .el-button.button{
    
    height: 50px;
    padding: 13px;
} */
  textarea {
    resize: none;
    border: 1px solid #e4e4e7;
    border-radius: 12px;
    width: 95%;
    height: 200px;
    padding: 16px;
  }
  .notify {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }
  .notify div {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .addCus {
    /* display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: repeat(2, 1fr);
    grid-row-gap: 16px; */
  }
</style>
