<template>
  <div id="app">
    <div class="start">
      <button v-on:click="addTicket">Add new</button>
      <div>
        <button
          v-for="(ticket, index) in tickets"
          :key="index"
          v-on:click="choseTicket(index)"
        >{{ index + 1 }}</button>
      </div>
      <div>
        <input
          v-if="this.activeTicket !== -1"
          v-model="currentInput"
          :ref="'inputNumber'"
          v-on:keyup="handleInputNumber"
        >
      </div>
    </div>
    <div class="ticket">
      <table v-for="(ticket, index) in tickets" :key="'table' + index">
        <tr v-for="(row, rowIndex) in ticket" :key="'row' + rowIndex">
          <td
            v-for="(data, dataIndex) in row"
            :key="'data' + dataIndex"
            :ref="index.toString() +rowIndex.toString() + dataIndex.toString()"
          >{{ data }}</td>
        </tr>
      </table>
    </div>
    <div class="result" v-if="tickets.length > 0">
      <div>
        <span>Result:</span>
        <span v-for="(result, index) in results" :key="index + 200">{{ result }}</span>
      </div>
      <div>
        <input v-model="currentResultInput" v-on:keyup="handleResultInput">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      tickets: [],
      activeTicket: -1,
      currentInput: "",
      results: [],
      currentResultInput: ""
    };
  },
  methods: {
    addTicket() {
      this.tickets.push([]);
      this.activeTicket = this.tickets.length - 1;
      this.$nextTick(() => {
         this.$refs['inputNumber'].focus();
      });
     
    },
    choseTicket(index) {
      this.activeTicket = index;
    },
    handleInputNumber(e) {
      if (e.keyCode === 13) {
        if (isNaN(parseInt(this.currentInput))) return;
        const activeTicket = this.tickets[this.activeTicket];

        if (activeTicket.length === 0) {
          activeTicket.push([]);
          activeTicket[0].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 1 && activeTicket[0].length < 5) {
          activeTicket[0].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 1 && activeTicket[0].length === 5) {
          activeTicket.push([]);
          activeTicket[1].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 2 && activeTicket[1].length < 5) {
          activeTicket[1].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 2 && activeTicket[1].length === 5) {
          activeTicket.push([]);
          activeTicket[2].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 3 && activeTicket[2].length < 5) {
          activeTicket[2].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 3 && activeTicket[2].length === 5) {
          activeTicket.push([]);
          activeTicket[3].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 4 && activeTicket[3].length < 5) {
          activeTicket[3].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 4 && activeTicket[3].length === 5) {
          activeTicket.push([]);
          activeTicket[4].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 5 && activeTicket[4].length < 5) {
          activeTicket[4].push(parseInt(this.currentInput));
          this.currentInput = "";
          return;
        }

        if (activeTicket.length === 5 && activeTicket[2].length === 5) {
          this.addTicket();
          return;
        }
      }
    },
    handleResultInput(e) {
      if (e.keyCode === 13) {
        if (isNaN(parseInt(this.currentResultInput))) return;

        for (let i = 0; i < this.tickets.length; i++) {
          for (let j = 0; j < this.tickets[i].length; j++) {
            for (let k = 0; k < this.tickets[i][j].length; k++) {
              if (this.tickets[i][j][k] === parseInt(this.currentResultInput)) {
                const refIndex = i.toString() + j.toString() + k.toString();
                const ref = this.$refs[refIndex][0];
                ref.style.backgroundColor = "red";
              }
            }
          }
        }

        this.results.push(parseInt(this.currentResultInput));
        this.currentResultInput = "";
      }
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.result {
  margin-top: 20px;
}

.ticket {
  display: flex;
}

td {
  display: inline-block;
  padding: 20px;
  border: solid 1px #ccc;
  width: 20px;
  height: 20px;
  margin-right: 2px;
}

table {
  padding-left: 20px;
}

table:first {
  padding-left: 0px;
}
</style>
