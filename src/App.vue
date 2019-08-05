<template>
  <div id="app">
    <div class="start">
      <button v-on:click="addTicket">Add new</button>
      <button v-on:click="clearTickets">Clear Tickets</button>
      <div>
        <button
          v-for="(ticket, index) in tickets"
          :key="index"
          v-on:click="choseTicket(index)"
          :class="{'active': activeTicket === index}"
        >{{ index + 1 }}</button>
      </div>
      <div>
        <input
          v-if="this.activeTicket !== -1"
          v-model="currentInput"
          :ref="'inputNumber'"
          v-on:keyup="handleInputNumber"
        />
      </div>
    </div>
    <div class="tickets">
      <div class="ticket" v-for="(ticket, index) in tickets" :key="'table' + index">
        <button v-on:click="deleteTicket(index)">Delete ticket {{index + 1}}</button>
        <table>
          <tr v-for="(row, rowIndex) in ticket" :key="'row' + rowIndex">
            <td
              v-for="(data, dataIndex) in row"
              :key="'data' + dataIndex"
              :ref="index.toString() +rowIndex.toString() + dataIndex.toString()"
            >
              <span>{{data}}</span>
              <input
                class="inlineEdit"
                v-bind:data-id="index+':'+rowIndex+':'+dataIndex"
                v-on:keyup="handleEditNumber"
              />
            </td>
          </tr>
        </table>
      </div>
    </div>
    <div class="result" v-if="tickets.length > 0">
      <div>
        <span>Result:</span>
        <span class="result" v-for="(result, index) in results" :key="index + 200">{{ result }}</span>
      </div>
      <div>
        <input v-model="currentResultInput" v-on:keyup="handleResultInput" />
      </div>
      <div>
        <button v-on:click="clearResults">Clear result</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  mounted() {
    if (localStorage.tickets) {
      this.tickets = JSON.parse(localStorage.tickets);
    }
  },
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
        this.$refs["inputNumber"].focus();
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
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 1 && activeTicket[0].length < 5) {
          activeTicket[0].push(parseInt(this.currentInput));
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 1 && activeTicket[0].length === 5) {
          activeTicket.push([]);
          activeTicket[1].push(parseInt(this.currentInput));
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 2 && activeTicket[1].length < 5) {
          activeTicket[1].push(parseInt(this.currentInput));
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 2 && activeTicket[1].length === 5) {
          activeTicket.push([]);
          activeTicket[2].push(parseInt(this.currentInput));
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 3 && activeTicket[2].length < 5) {
          activeTicket[2].push(parseInt(this.currentInput));
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 3 && activeTicket[2].length === 5) {
          activeTicket.push([]);
          activeTicket[3].push(parseInt(this.currentInput));
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 4 && activeTicket[3].length < 5) {
          activeTicket[3].push(parseInt(this.currentInput));
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 4 && activeTicket[3].length === 5) {
          activeTicket.push([]);
          activeTicket[4].push(parseInt(this.currentInput));
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 5 && activeTicket[4].length < 5) {
          activeTicket[4].push(parseInt(this.currentInput));
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
          return;
        }

        if (activeTicket.length === 5 && activeTicket[2].length === 5) {
          this.addTicket();
          this.tickets[this.tickets.length - 1].push([]);
          this.tickets[this.tickets.length - 1][0].push(
            parseInt(this.currentInput)
          );
          this.currentInput = "";
          localStorage.tickets = JSON.stringify(this.tickets);
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
    },
    clearResults() {
      this.results = [];
      for (let i = 0; i < this.tickets.length; i++) {
        for (let j = 0; j < this.tickets[i].length; j++) {
          for (let k = 0; k < this.tickets[i][j].length; k++) {
            const refIndex = i.toString() + j.toString() + k.toString();
            const ref = this.$refs[refIndex][0];
            ref.style.backgroundColor = "";
          }
        }
      }
    },
    clearTickets() {
      this.tickets = [];
      this.activeTicket = -1;
      localStorage.tickets = JSON.stringify(this.tickets);
    },
    deleteTicket(index) {
      this.tickets.splice(index, 1);
      this.activeTicket = -1;
      localStorage.tickets = JSON.stringify(this.tickets);
    },
    handleEditNumber(e) {
      if (e.keyCode === 13) {
        if (isNaN(parseInt(e.target.value))) return;
        const idSet = e.target.dataset.id.split(":");
        this.$set(
          this.tickets[parseInt(idSet[0])][parseInt(idSet[1])],
          parseInt(idSet[2]),
          parseInt(e.target.value)
        );
        e.target.value = "";
        localStorage.tickets = JSON.stringify(this.tickets);
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

.tickets {
  display: flex;
  flex-wrap: wrap;
}

td {
  display: inline-block;
  padding: 20px;
  border: solid 1px #ccc;
  width: 25px;
  height: 25px;
  margin-right: 2px;
}

table {
  margin-left: 20px;
  border: 2px solid #e67e22;
}

table:first {
  padding-left: 0px;
}

.result {
  margin: 2px;
}

.inlineEdit {
  width: 20px;
}
.ticket {
  margin-top: 10px;
}
.active {
  background: yellow;
}
</style>
