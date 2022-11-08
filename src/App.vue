<template>
  <form>
    <label for="iban">IBAN</label>
    <input type="text" name="iban" v-model="iban" @change="check" />
    <!-- <button @click.prevent="check">Check</button> -->
  </form>
  <div class="responses">
    <span class="correct" v-if="isIbanCorrect.allChecksPassed === true"
      >The iban is correct</span
    >
    <span
      class="not-correct"
      v-if="
        isIbanCorrect.allChecksPassed != null &&
        isIbanCorrect.allChecksPassed !== true
      "
      >The iban is not correct</span
    >
  </div>
  <div v-if="isIbanCorrect.allChecksPassed === true" class="bank-data">
    <table>
      <tr>
        <td><span class="table-title">IBAN</span></td>
        <td>{{ isIbanCorrect.iban }}</td>
      </tr>
      <tr>
        <td><span class="table-title">BIC</span></td>
        <td>{{ isIbanCorrect.bic }}</td>
      </tr>
      <tr>
        <td><span class="table-title">Bank Name</span></td>
        <td>{{ isIbanCorrect.bankName }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      iban: "",
      isIbanCorrect: "",
    };
  },
  methods: {
    async check() {
      console.log(this.iban);
      if (this.iban.length > 5) {
        const options = {
          method: "POST",
          url: "/api/v5/check-iban",
          headers: {
            "Content-Type": "application/json",
            "API-Key-Id": import.meta.env.VITE_API_KEY_ID,
            "API-Key-Secret": import.meta.env.VITE_API_KEY_SECRET,
            //prettier-ignore
            "Accept": "application/json",
            "Access-Control-Allow-Origin": "*",
          },
          data: { iban: this.iban },
        };
        try {
          const response = await axios.request(options);

          this.isIbanCorrect = response.data;

          console.log("THIS", this.isIbanCorrect);
        } catch (e) {
          console.error(e);
        }
      } else {
        return;
      }
    },
  },
};
</script>

<style scoped>
form {
  /* height: 100px; */
  height: 50px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}
.responses {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.correct {
  font-weight: bold;
  color: rgb(2, 172, 2);
}
.not-correct {
  font-weight: bold;
  color: rgb(235, 2, 2);
}

table,
td,
tr {
  border: 1px solid;
  border-collapse: collapse;
  padding: 10px;
}

.table-title {
  font-weight: bold;
  color: rgb(3, 3, 104);
}
.bank-data {
  margin-top: 20px;
}
</style>
