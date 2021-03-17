# Vuetify-DateTimePicker
Component based on Vuetify datepicker and input so you can use v-model and Date.

You simply import this component, and set the v-model to a Date (Date Object - not string). You can simply customize the component to change the look of the input or make it readonly, the component needs Vuetify to be installed because it uses the Vuetify v-date-picker and the Vuetify v-text-field and the v-menu. The time will be reserved when changing the date with the datepicker, and if you enter a datetime/date/time it will check for formatting errors and try to extract the date - (resetting the time, will update this later by trying to parse the hour/minutes/seconds).
