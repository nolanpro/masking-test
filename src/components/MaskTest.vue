<template>
  <div>
    <b-container>
      <h3>Currency</h3>
      <b-form-select v-model="selected" :options="options"></b-form-select>
      <b-form-input v-model="currencyValue" ref="currencyInput"></b-form-input>
      <div class="mt-2">Value: <pre>{{ currencyUnformattedValue }}</pre></div>
      
      <h3>Percentage</h3>
      <b-form-input v-model="percentValue" ref="percentInput"></b-form-input>
      <div class="mt-2">Value: <pre>{{ percentUnformattedValue }}</pre></div>
      
      <h3>Date</h3>
      <b-form-input v-model="dateValue" ref="dateInput"></b-form-input>
      <div class="mt-2">Value: <pre>{{ dateValue }}</pre></div>
      
      <h3>Date Time</h3>
      <b-form-input v-model="dateTimeValue" ref="dateTimeInput"></b-form-input>
      <div class="mt-2">Value: <pre>{{ dateTimeValue }}</pre></div>
    </b-container>
  </div>
</template>

<script>
import Inputmask from "inputmask";
import currency from "../currency.json"

export default {
  data() {
    return {
      currencyValue: '',
      currencyUnformattedValue: null,
      percentValue: '',
      percentUnformattedValue: null,
      dateValue: '',
      dateTimeValue: '',

      selected: 'USD',
      currencyConfig: {
        prefix: "$ ",
        suffix: " USD",
        radixPoint: ".",
        groupSeparator: ",",
      },
      percentConfig: {
        alias: "numeric",
        digits: 2,
        suffix: " %",
        groupSeparator: ",",
        max: null,
      },
      dateConfig: {
        alias: 'date',
        inputFormat: "dd/mm/yyyy",
      },
      dateTimeConfig: {
        alias: 'date',
        inputFormat: "dd/mm/yyyy HH:MM",
      }
    }
  },
  mounted() {
    this.loadMasks();
  },
  computed: {
    currencyInput() {
      return this.$refs.currencyInput.$el;
    },
    percentInput() {
      return this.$refs.percentInput.$el;
    },
    dateInput() {
      return this.$refs.dateInput.$el;
    },
    dateTimeInput() {
      return this.$refs.dateTimeInput.$el;
    },
    options() {
      return currency.map(val => {
        return { value: val.code, text: val.code + ' - ' + val.name };
      });
    },
  },
  methods: {
    loadMasks() {
      Inputmask("currency", this.currencyConfig).mask(this.currencyInput);
      Inputmask("percentage", this.percentConfig).mask(this.percentInput);
      Inputmask("datetime", this.dateConfig).mask(this.dateInput);
      Inputmask("datetime", this.dateTimeConfig).mask(this.dateTimeInput);
    }
  },
  watch: {
    currencyValue() {
      this.currencyUnformattedValue = parseFloat(
        this.currencyInput.inputmask.unmaskedvalue().replace(",",".")
      );
    },
    percentValue() {
      this.percentUnformattedValue = parseFloat(
        this.percentInput.inputmask.unmaskedvalue()
      ) / 100;
    },
    selected() {
      const conf = currency.find(val => val.code === this.selected);
      this.currencyConfig.prefix = conf.symbol + ' ';
      this.currencyConfig.suffix = ' ' + conf.code;
      
      const parts = conf.format.match(/([^#])/g);
      this.currencyConfig.radixPoint = parts[parts.length - 1];
      if (parts.length > 1) {
        this.currencyConfig.groupSeparator = parts[0];
      } else {
        this.currencyConfig.groupSeparator = "";
      }
      
      this.loadMasks();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
