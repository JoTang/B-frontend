<template>
  <form @submit.prevent="submit">
    <div class="form-group">
      <label for="amountInput">金额</label>
      <input id="amountInput" type="number" step="0.01" class="form-control" v-model.number="amount" required>
    </div>
    <div class="form-group">
      <label for="descriptionInput">描述</label>
      <input id="descriptionInput" type="text" class="form-control" v-model="description" required>
    </div>
    <div class="form-group">
      <label for="timeInput">时间</label>
      <div class="input-group date form_date" id="timeInputGroup">
        <input id="timeInput" type="text" class="form-control" v-model="rawTime" readonly data-input>
        <span class="input-group-addon" data-toggle>
          <span class="glyphicon glyphicon-calendar"></span>
        </span>
        <span class="input-group-addon" data-clear>
          <span class="glyphicon glyphicon-remove"></span>
        </span>
      </div>
    </div>
    <button type="submit" class="btn btn-default">提交</button>
    <span class="text-success" v-if="showSuccessSign">
      <span class="glyphicon glyphicon-ok"></span> 成功
    </span>
    <span class="text-danger" v-if="showFailSign">
      <span class="glyphicon glyphicon-remove"></span> 失败
    </span>
  
  </form>
</template>

<script>
import 'flatpickr';
import 'flatpickr/dist/flatpickr.min.css';

export default {
  /* eslint-disable no-undef */
  data() {
    return {
      amount: 0,
      description: '',
      rawTime: '',
      showSuccessSign: false,
      showFailSign: false,
      url: process.env.URL,
    };
  },
  computed: {
    formData() {
      if (isNaN(this.time)) {
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
    time() {
      return Date.parse(this.rawTime);
    },
  },
  methods: {
    submit() {
      const myHeaders = new Headers();
      const myBody = JSON.stringify(this.formData);
      myHeaders.append('Content-Type', 'application/json');
      fetch(this.url, {
        method: 'POST',
        body: myBody,
        headers: myHeaders,
      }).then((res) => {
        if (res.ok) {
          this.showSuccessSign = true;
          $('form')[0].reset();
          this.amount = 0;
          this.description = '';
        } else {
          this.showFailSign = true;
        }
      });
    },
  },
  mounted() {
    $('#timeInputGroup').flatpickr({
      enableTime: true,
      wrap: true,
      defaultDate: new Date(),
      time_24hr: true,
    });
    this.$set(this.$data, 'rawTime', $('#timeInput').val());
  },

};
</script>

<style scoped>

</style>
