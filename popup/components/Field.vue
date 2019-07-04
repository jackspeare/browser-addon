<template>
  <v-layout row justify-center align-center>
    <v-flex class="text-truncate my-2 text-xs-right">
      <v-tooltip bottom>
        <span slot="activator" @click="toggleReveal">{{displayValue}}</span>
        <span>{{tooltip}}</span>
      </v-tooltip>
    </v-flex>

    <v-flex class="my-0" shrink>
      <v-tooltip left>
        <v-btn slot="activator" small icon class="my-0">
          <v-icon small>content_copy</v-icon>
        </v-btn>
        <span>Copy value to clipboard</span>
      </v-tooltip>
    </v-flex>
  </v-layout>
</template>

<script lang="ts">
const protectedText = "**********";
export default {
  props: ["field"],
  data: () => ({
    revealed: false
  }),
  computed: {
    displayValue: function(this: any) {
      return this.field.type === "password" && !this.revealed
        ? protectedText
        : this.field.value;
    },
    tooltip: function(this: any) {
      return (
        this.field.name +
        ": " +
        (this.displayValue === protectedText
          ? "Click to reveal/hide"
          : this.field.value)
      );
    }
  },
  methods: {
    toggleReveal: function(this: any) {
      this.revealed = !this.revealed;
    }
  }
};
</script>