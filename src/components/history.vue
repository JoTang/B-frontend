<template>
  <div>
    <table class="table table-hover">
      <thead>
        <tr>
          <th>编号</th>
          <th>金额</th>
          <th>时间</th>
          <th>描述</th>
          <th>创建者IP</th>
          <th>创建者User-Agent</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="record in sorted">
          <td>{{record.id}}</td>
          <td>{{record.amount}}</td>
          <td>{{record.time}}</td>
          <td>{{record.description}}</td>
          <td>{{record.ip}}</td>
          <td>{{record.ua}}</td>
        </tr>
      </tbody>
    </table>
    <p class="bg-info">当前剩余经费数量:{{balance}}</p>
    <button class="btn btn-default" @click="loadHistory"><span class="glyphicon glyphicon-refresh"></span> 刷新</button>
    <span class="text-success" v-if="showSuccessSign"><span class="glyphicon glyphicon-ok"></span>成功</span>
    <span class="text-danger" v-if="showFailSign"><span class="glyphicon glyphicon-remove"></span>失败</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      historyData: [],
      showSuccessSign: false,
      showFailSign: false,
    };
  },
  computed: {
    balance() {
      let result = 0;
      this.historyData.forEach((record) => {
        result += record.amount;
      });
      return result;
    },
    sorted() {
      return this.historyData.sort((a, b) => (a.id - b.id));
    },
  },
  methods: {
    loadHistory() {
      this.showSuccessSign = false;
      this.showSuccessSign = false;
      fetch('http://fa527e10.jotang.party/transaction').then((res) => {
        if (res.ok) {
          this.historyData = res.body.json;
          this.historyData.forEach((record) => {
            const time = new Date(record.time);
            // eslint-disable-next-line no-param-reassign
            record.time = time.toString();
          });
          this.showSuccessSign = true;
        } else {
          this.showFailSign = true;
        }
      });
    },
  },
  mounted() {
    this.loadHistory();
  },

};

</script>

<style scoped>

</style>
