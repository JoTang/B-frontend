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
        <input id="timeInput" type="text" class="form-control" readonly>
        <span class="input-group-addon">
            <span class="glyphicon glyphicon-calendar"></span>
        </span>
      </div>
    </div>
    <button type="submit" class="btn btn-default">提交</button>
    <span class="text-success" v-if="showSuccessSign"><span class="glyphicon glyphicon-ok">成功</span></span>
    <span class="text-danger" v-if="showFailSign"><span class="glyphicon glyphicon-remove">失败</span></span>
  
  </form>
</template>

<script>
export default {
  data() {
    return {
      amount: 0,
      description: '',
      time: Date.now(),
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
          // eslint-disable-next-line no-undef
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
    // eslint-disable-next-line no-undef
    $('#timeInputGroup').datetimepicker({
      format: 'yyyy-mm-dd hh:ii',
      autoclose: true,
      todayBtn: true,
      todayHighlight: true,
      language: 'zh-CN',
    }).on('changeDate', (e) => {
      this.time = e.date.valueOf();
    });
  },

};
</script>

<style scoped>

</style>
