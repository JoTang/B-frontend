<template>
  <div>
    <tab></tab>
  
    <div class="tab-content">
      <newrecord-panel role="tabpanel"
                       class="tab-pane fade in active"
                       id="newrecord-panel"></newrecord-panel>
      <history-panel role="tabpanel"
                     class="tab-pane fade"
                     id="history-panel"></history-panel>
    </div>
  </div>
</template>

<script>
import tab from './components/nav';
import newrecordPanel from './components/newrecord';
import historyPanel from './components/history';

export default {
  components: {
    tab,
    newrecordPanel,
    historyPanel,
  },
};
</script>

<style>

</style>
