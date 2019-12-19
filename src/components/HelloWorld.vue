<template>
  <div class="hello">
    <div class="row">
      <div class="col-6 text-right"><h4>Letzte Periode:</h4></div>
      <div class="col-6 text-left">
        <date-picker v-model="date1" format="DD.MM.YYYY" valueType="x"></date-picker>
      </div>
    </div>
    <div class="row">
      <div class="col-6 text-right"><h4>Vorletzte Periode:</h4></div>
      <div class="col-6 text-left">
        <date-picker v-model="date2" format="DD.MM.YYYY" valueType="x"></date-picker>
      </div>
    </div>
    <div class="row">
      <div class="col-6 text-right"><h4>Vorvorletzte Periode:</h4></div>
      <div class="col-6 text-left">
        <date-picker v-model="date3" format="DD.MM.YYYY" valueType="x"></date-picker>
      </div>
    </div>
    <div class="mt-4 mb-4 d-flex flex-row justify-content-center">
      <button @click="calculate" class="btn btn-primary">Auswerten</button>
    </div>
    <div v-if="calculated">
      <div class="row">
        <div class="col-6 text-right">Follikelphase 1: </div>
        <div class="col-6 text-left">{{ formatDate(follikelphase1start) + ' - ' + formatDate(follikelphase1end) }}</div>
      </div>
      <div class="row">
        <div class="col-6 text-right">Lutealphase 1: </div>
        <div class="col-6 text-left">{{  formatDate(lutealphase1start) + ' - ' + formatDate(lutealphase1end)  }}</div>
      </div>
      <div class="row">
        <div class="col-6 text-right">Follikelphase 2: </div>
        <div class="col-6 text-left">{{formatDate(follikelphase2start) + ' - ' + formatDate(follikelphase2end)}}</div>
      </div>
      <div class="row">
        <div class="col-6 text-right">Lutealphase 2: </div>
        <div class="col-6 text-left">{{ formatDate(lutealphase2start) + ' - ' + formatDate(lutealphase2end) }}</div>
      </div>
      <div class="row">
        <div class="col-6 text-right">Nächste Periode: </div>
        <div class="col-6 text-left">{{ formatDate(nextPeriod) }}</div>
      </div>
      <div class="row">
        <div class="col-6 text-right">Übernächste Periode: </div>
        <div class="col-6 text-left">{{ formatDate(secondNextPeriod) }}</div>
      </div>
      <div class="row">
        <div class="col-6 text-right">Letzte Periodendauer: </div>
        <div class="col-6 text-left">{{ Math.floor(lastPeriod / 86400000) }}</div>
      </div>
      <div class="row">
        <div class="col-6 text-right">Vorletzte Periodendauer: </div>
        <div class="col-6 text-left">{{ Math.floor(secondLastPeriod / 86400000) }}</div>
      </div>
      <div class="row">
        <div class="col-6 text-right">Durchschnittliche Periodendauer: </div>
        <div class="col-6 text-left">{{ Math.floor(average / 86400000) }}</div>
      </div>
    </div>
  </div>
</template>

<script>
  import DatePicker from 'vue2-datepicker';
  import 'vue2-datepicker/index.css';

export default {
  name: 'HelloWorld',
  components: { DatePicker },
  data() {
    return {
      calculated: false,
      date1: null,
      date2: null,
      date3: null,
      lastPeriod: null,
      secondLastPeriod: null,
      average: null,
      nextPeriod: null,
      secondNextPeriod: null,
      follikelphase1start: null,
      follikelphase1end: null,
      lutealphase1start: null,
      lutealphase1end: null,
      follikelphase2start: null,
      follikelphase2end: null,
      lutealphase2start: null,
      lutealphase2end: null,
    };
  },
  props: {
    msg: String
  },
  methods: {
    calculate() {
      if (this.date1 !== null && this.date2 !== null && this.date3 !== null) {
        this.date1 = parseInt(this.date1)
        this.date2 = parseInt(this.date2)
        this.date3 = parseInt(this.date3)
        this.lastPeriod = (this.date1 - this.date2)
        this.secondLastPeriod = (this.date2 - this.date3)
        this.average = (this.lastPeriod + this.secondLastPeriod) / 2
        this.nextPeriod = this.date1 + this.average
        this.secondNextPeriod = this.date1 + (this.average * 2)
        this.follikelphase1start = this.nextPeriod - (this.average - (5 * 86400000))
        this.follikelphase1end = this.nextPeriod - (17 * 86400000)
        this.lutealphase1start = this.nextPeriod - (8 * 86400000)
        this.lutealphase1end = this.nextPeriod - (3 * 86400000)
        this.follikelphase2start = this.secondNextPeriod - (this.average - (5 * 86400000))
        this.follikelphase2end = this.secondNextPeriod - (17 * 86400000)
        this.lutealphase2start = this.secondNextPeriod - (8 * 86400000)
        this.lutealphase2end = this.secondNextPeriod - (3 * 86400000)
        this.calculated = true
      }
    },
    formatDate(unixTimestamp) {
      const date = new Date(unixTimestamp)
      return ("0" + date.getDate()).slice(-2) + '.' + ("0" + (date.getMonth() + 1)).slice(-2) + '.' + date.getFullYear()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
