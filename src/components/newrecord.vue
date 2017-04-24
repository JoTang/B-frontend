<template>
  <form @submit.prevent="submit">
    <div class="form-group">
      <label for="amountInput">金额</label>
      <input id="amountInput"
             type="number"
             step="0.01"
             class="form-control"
             v-model.number="amount" required>
    </div>
    <div class="form-group">
      <label for="descriptionInput">描述</label>
      <input id="descriptionInput"
             type="text"
             class="form-control"
             v-model="description" required>
    </div>
    <div class="form-group">
      <label for="timeInput">时间</label>
      <input id="timeInput"
             type="number"
             class="form-control"
             v-model.number="time">
    </div>
    <button type="submit"
            class="btn btn-default">提交</button>
    <span class="text-success"
          v-if="showSuccessSign"><span class="glyphicon glyphicon-ok">成功</span></span>
    <span class="text-danger"
          v-if="showFailSign"><span class="glyphicon glyphicon-remove">失败</span></span>
  
  </form>
</template>

<script>
export default {
  data() {
    return {
      amount: 0,
      description: '',
      time: '',
      showSuccessSign: false,
      showFailSign: false,
    };
  },
  computed: {
    formData() {
      if (typeof (this.time) !== 'number') {
        return {
          amount: this.amount,
          description: this.description,
        };
      }
      return {
        amount: this.amount,
        description: this.description,
        time: this.time,
      };
    },
  },
  methods: {
    submit() {
      const myHeaders = new Headers();
      const myBody = JSON.stringify(this.formData);
      myHeaders.append('Content-Type', 'application/json');
      fetch('http://fa527e10.jotang.party/transaction', {
        method: 'POST',
        body: myBody,
        headers: myHeaders,
      }).then((res) => {
        if (res.ok) {
          this.showSuccessSign = true;
        } else {
          this.showFailSign = true;
        }
      });
    },

  },

};
</script>

<style scoped>

</style>
