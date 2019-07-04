<template>
  <v-hover>
    <v-card slot-scope="{ hover }" :elevation="`${hover ? 12 : 3}`" class="my-2">
      <v-layout row justify-center align-center :style="`${hover ? 'cursor: pointer' : ''}`">
        <v-flex class="text-truncate">
          <v-hover>
            <v-card-title class="pl-5 ml-2 py-3 subheading" :style="titleStyle">
              <span class="text-truncate">{{entry.title}}</span>
              <v-flex
                :style="`${show ? 'visibility:hidden' : ''}`"
                class="text-truncate caption pr-2 pt-1 py-1"
              >{{entry.usernameValue}}</v-flex>
            </v-card-title>
          </v-hover>
        </v-flex>
        <v-flex class="my-0" v-if="hover">
          <v-btn icon @click="show = !show">
            <v-icon>{{ show ? 'keyboard_arrow_down' : 'keyboard_arrow_up' }}</v-icon>
          </v-btn>
        </v-flex>
      </v-layout>
      <v-card-text class="py-0 text-truncate">
        <v-slide-y-transition>
          <div v-show="show">
            <Field v-for="f of entry.fields" :key="f.value" :field="f"/>
            <!-- TODO new uuids for keys? -->
            <v-layout row justify-space-between align-center>
              <v-flex class="text-truncate">
                <v-tooltip top :disabled="false">
                  <!-- TODO: depend on length of caption - rough estimate but better than nothing -->
                  <v-layout row justify-left align-center slot="activator">
                    <v-icon small class="py-1 pl-0 pr-2">folder</v-icon>
                    <span class="text-truncate caption py-1">{{entry.groupPath}}</span>
                  </v-layout>
                  <span>{{entry.groupPath}}</span>
                </v-tooltip>
                <v-tooltip top>
                  <v-layout row justify-left align-center slot="activator">
                    <v-icon small class="py-1 pl-0 pr-2">cloud</v-icon>
                    <span class="text-truncate caption py-1">{{entry.domain}}</span>
                  </v-layout>
                  <span>{{entry.URL}}</span>
                </v-tooltip>
              </v-flex>

              <v-flex shrink class="ma-2">
                <v-btn fab small left>
                  <v-icon>edit</v-icon>
                </v-btn>
              </v-flex>
            </v-layout>
          </div>
        </v-slide-y-transition>
      </v-card-text>
    </v-card>
  </v-hover>
</template>

<script lang="ts">
import Field from "./Field.vue";
export default {
  props: ["show", "entry"],
  computed: {
    titleStyle: function(this: any) {
      return (
        "background-position:16px calc(50% - 1px); background-image: " +
        this.entry.iconURI
      );
    }
  },
  components: { Field }
};
</script>