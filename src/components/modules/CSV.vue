<template>
  <input @change="fileChange" type="file">
  <button class="button button-outline" @click="clear">クリア</button>
  <table>
    <thead>
      <tr>
        <th>日付</th>
        <th>購入元</th>
        <th>金額</th>
        <th>分割回数</th>
        <th>支払回数</th>
        <th>支払合計金額</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, key) in workers" :key="key">
        <td>{{item.date}}</td>
        <td>{{item.name}}</td>
        <td>{{item.pay}}</td>
        <td>{{item.count_total}}</td>
        <td>{{item.count_current}}</td>
        <td>{{item.pay_current}}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  data() {
    return {
    workers: [],
    }
  },
  methods: {
    fileChange: function (e) {
      const file = e.target.files[0];
      const reader = new FileReader();
      const workers = [];
      
      const loadFunc = () => {
        const lines = reader.result.split("\n");

        lines.forEach((element) => {
          const workerData = element.split(",");
          if (workerData.length == 3) return;
          console.log(workerData);
          const worker = {
            date: workerData[0],
            name: workerData[1],
            pay:workerData[2],
            count_total: workerData[3],
            count_current: workerData[4],
            pay_current: workerData[5]
          };
          workers.push(worker);
        });
        this.workers = workers;

      };

      // onloadはreadAsBinaryStringでファイルを読み込んだ後に実行されます.
      reader.onload = loadFunc;

      // reader.readAsBinaryString(file);
      reader.readAsText(file, 'Shift_JIS');
    },
    clear(){
      this.workers = [];
    },
  },
};
</script>

<style scoped>
th{
  position: sticky;
  top: 0;
  z-index: 114514;
}
</style>