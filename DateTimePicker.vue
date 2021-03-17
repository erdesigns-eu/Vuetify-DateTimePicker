<template>
  <v-menu v-model="menu" :close-on-content-click="false" transition="scale-transition" offset-y max-width="290px" min-width="auto">
    <template v-slot:activator="{ on, attrs }">
      <v-text-field v-model="dateFormatted" v-bind="attrs" v-on="on" @blur="updateDateFormatted"></v-text-field>
    </template>
    <v-date-picker v-model="dateStr" no-title @input="menu = false"></v-date-picker>
  </v-menu>
</template>

<script>
/* eslint-disable */

export default {
  name: 'DateTimePicker',
  props: {
    date: Date
  },
  model: {
      prop: 'date',
      event: 'blur'
  },
  data () {
    return {
      d: this.date,
      menu: false,
      dstr: ''
    }
  },
  methods: {
    isValidDate (d) {
      return d instanceof Date && !isNaN(d);
    },
    updateDateFormatted () {
      if (this.dstr !== '') {
        let d = new Date(this.dstr);
        if (this.isValidDate(d)) {
          this.d = d;
        } else {
          this.d = new Date(this.parseDate);
          this.dstr = `${this.d.getFullYear()}-${this.d.getMonth()}-${this.d.getDate()}`;
        }
        this.$emit('blur', this.d);
      }
    },
    updateDate () {
      this.d = new Date()
    }
  },
  computed: {
    dateStr: {
      get () {
        let d = this.d;
        return `${d.getFullYear()}-${String(d.getMonth() +1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')}`;
      },
      set (date) {
        let d = new Date(date);
        d.setHours(this.d.getHours(), this.d.getMinutes(), this.d.getSeconds());
        this.d = d;
        this.$emit('blur', this.d);
      }
    },
    dateFormatted: {
      get () {
        let d = this.d;
        return `${d.getFullYear()}-${String(d.getMonth() +1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')} ${String(d.getHours()).padStart(2, '0')}:${String(d.getMinutes()).padStart(2, '0')}:${String(d.getSeconds()).padStart(2, '0')}`;
      },
      set (date) {
        this.dstr = date;
      }
    },
    parseDate () {
      let d = this.dstr;
      let day, month, year, result, dateSplitted;

      result = d.match("[0-9]{2}([\-/ \.])[0-9]{2}[\-/ \.][0-9]{4}");
      if (null != result) {
          dateSplitted = result[0].split(result[1]);
          day = dateSplitted[0];
          month = dateSplitted[1];
          year = dateSplitted[2];
      }
      result = d.match("[0-9]{4}([\-/ \.])[0-9]{2}[\-/ \.][0-9]{2}");
      if (null != result) {
          dateSplitted = result[0].split(result[1]);
          day = dateSplitted[2];
          month = dateSplitted[1];
          year = dateSplitted[0];
      }

      if (month > 12) {
          aux = day;
          day = month;
          month = aux;
      }

      return `${year}-${month}-${day}`;
    }
  }
};
</script>
