<template>
  <div class="table-responsive">
    <table class="table table-hover">
      <thead>
        <tr>
          <th>编号</th>
          <th>金额</th>
          <th>时间</th>
          <th>描述</th>
          <th>创建者IP</th>
          <th>创建者浏览器</th>
          <th>创建者OS</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="record in processedData" :key="record.id">
          <td>{{record.id}}</td>
          <td>{{record.amount}}</td>
          <td>{{record.time}}</td>
          <td>{{record.description}}</td>
          <td>{{record.ip}}</td>
          <td>{{record.browser}}</td>
          <td>{{record.os}}</td>
        </tr>
      </tbody>
    </table>
    <p class="bg-info">当前剩余经费数量:{{balance}}</p>
    <button class="btn btn-default" @click="loadHistory">
      <span class="glyphicon glyphicon-refresh"></span> 刷新
    </button>
    <span class="text-success" v-if="showSuccessSign">
      <span class="glyphicon glyphicon-ok"></span> 成功
    </span>
    <span class="text-danger" v-if="showFailSign">
      <span class="glyphicon glyphicon-remove"></span> 失败
    </span>
  </div>
</template>

<script>
import timeago from 'timeago.js';
import UAParser from 'ua-parser-js';

export default {
  data() {
    return {
      historyData: [],
      showSuccessSign: false,
      showFailSign: false,
      url: process.env.URL,
    };
  },
  computed: {
    balance() {
      return this.historyData.reduce((acc, cur) => (acc + cur.amount), 0);
    },
    processedData() {
      return this.historyData.sort((a, b) => b.time - a.time).map((record) => {
        const processedRecord = record;
        // eslint-disable-next-line no-undef
        const ua = new UAParser().setUA(processedRecord.ua);
        processedRecord.browser = `${ua.getBrowser().name}/${ua.getBrowser().version}`;
        processedRecord.os = `${ua.getOS().name} ${ua.getOS().version}`;
        processedRecord.time = timeago().format(processedRecord.time, 'zh_CN');
        return processedRecord;
      });
    },
  },
  methods: {
    loadHistory() {
      this.showSuccessSign = false;
      this.showFailSign = false;
      // eslint-disable-next-line no-undef
      $.ajax(this.url, {
        success: (data) => {
          this.showSuccessSign = true;
          this.historyData = data;
        },
        error: () => {
          this.showFailSign = true;
        },
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
