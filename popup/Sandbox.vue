
<template>
  <v-app id="inspire" :dark="darkTheme">
    <v-content>
      <v-container fluid fill-height>
        <v-layout row wrap justify-center align-top>
          <v-flex xs12>
            <v-text-field solo placeholder="Search..."></v-text-field>

            <v-hover>
              <v-card slot-scope="{ hover }" :elevation="`${hover ? 12 : 3}`">
                <v-layout
                  row
                  justify-center
                  align-center
                  :style="`${hover ? 'cursor: pointer' : ''}`"
                >
                  <v-flex class="text-truncate">
                    <v-hover>
                      <v-card-title
                        class="pl-5 ml-2 py-3 subheading"
                        style="background-position:16px calc(50% - 1px); background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAgY0hSTQAAeiYAAICEAAD6AAAAgOgAAHUwAADqYAAAOpgAABdwnLpRPAAAAgxJREFUOI2lkk9Ik2Ecxz/P8+7Pu9xKmRlNmYIgZSpGVhiO1qFDdOgWQVAEZnQzDx2mhyAN8tIluhd08RYUhLJhQUU1KYiiS7mgckJL073b3N73fTq8S13bIel7ep4fz/f5fb+/708AMDydRIoDbBW2PSC48vg1QuvbMrkM+T9kAFfNqgKXrnGmu4l6n4tHHzPMLxogRQ0FVWTF1Pke0qMD7Kxzs5QzuXu6k8x4FOmufl6pwFbcO9vFwsoajWOz6x3vJxdo31VHbjyKfjUOYkOJRv+5awBdoQBvRw6zb7ef6O1kpVwBS0YJAfS2BHj55df6J4KRGdXTHCBxaT+hiecU8wX6PPMkzQ5nGJvlejVKN47xI1uk+9Yr0ssFJEoxPdhLaOIZZjHH5+BlpgKTXPAmqPesEGt7AMrxbhYsxPAMbTdf8H1sAAS4sKFpu5di3gTpw0YgULyxW/kWGQIEOUvn6dpBrh96x8mHRzFyJSYTKY53BJFIwYd0lpbGbRVyDcuDqZwZJ7Nh4qfiRJoXudj5CYBwg87PvIlEQP+dOVKxI+wJ+f9y7aBkSyzlDO2r4WOov4UTe4PMpZadPVg1SuixWTyapKB0LCStWgbD8mEpjXY9w2rRja0EYX+O8A4vDaNPQAonhcp+0skNVQ6hfP5zF4DYoLhQtkJsDt2uyL8C1ZuMRBGpYfvfoNT734y1s7OXU2CAAAAAAElFTkSuQmCC');"
                      >
                        <span
                          class="text-truncate"
                        >Title sdfkjhsdfkljdf s dfl;kjsdhfsdfjkdjhksdfh dsfdsfsdfgsdfg</span>
                          <v-flex
                            :style="`${show ? 'visibility:hidden' : ''}`"
                            class="text-truncate caption pr-2 pt-1"
                          >Username.emailaddress@emailaddress.com.longishish</v-flex>
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
                  <!-- <v-layout row justify-center align-center >
                <v-flex class="text-truncate">
        Username.emailaddress@emailaddress.com.longish
                </v-flex>
                <v-flex>
                  <v-btn icon @click="show = !show">
            <v-icon>{{ show ? 'keyboard_arrow_down' : 'keyboard_arrow_up' }}</v-icon>
          </v-btn>
                </v-flex>
                  </v-layout>-->
                  <v-slide-y-transition>
                    <div v-show="show">
                      <v-layout row justify-center align-center>
                        <v-flex class="text-truncate my-2 text-xs-right">
                          <v-tooltip bottom>
                            <span slot="activator">Username.emailaddress@emailaddress.com.longish</span>
                            <span>Username: Username.emailaddress@emailaddress.com.longish</span>
                          </v-tooltip>
                        </v-flex>

                        <v-flex class="my-0" shrink>
                          <v-tooltip left>
                            <v-btn slot="activator" small icon class="my-0">
                              <v-icon small>content_copy</v-icon>
                            </v-btn>
                            <span>Copy username to clipboard</span>
                          </v-tooltip>
                        </v-flex>
                      </v-layout>

                      <v-layout row justify-center align-center>
                        <v-flex class="text-truncate my-2 text-xs-right">
                          <v-tooltip bottom>
                            <span slot="activator">**********</span>
                            <div>
                              <span>Password</span>
                              <br>
                              <span>Click to reveal/hide</span>
                            </div>
                          </v-tooltip>
                        </v-flex>

                        <v-flex class="my-0" shrink>
                          <v-tooltip left>
                            <v-btn slot="activator" small icon class="my-0">
                              <v-icon small>content_copy</v-icon>
                            </v-btn>
                            <span>Copy password to clipboard</span>
                          </v-tooltip>
                        </v-flex>
                      </v-layout>

                      <v-layout row justify-center align-center>
                        <v-flex class="text-truncate my-2 text-xs-right">
                          <v-tooltip bottom>
                            <span slot="activator">**********</span>
                            <div>
                              <span>Other name 1</span>
                              <br>
                              <span>Click to reveal/hide</span>
                            </div>
                          </v-tooltip>
                        </v-flex>

                        <v-flex class="my-0" shrink>
                          <v-tooltip left>
                            <v-btn slot="activator" small icon class="my-0">
                              <v-icon small>content_copy</v-icon>
                            </v-btn>
                            <span>Copy "Other name 1" to clipboard</span>
                          </v-tooltip>
                        </v-flex>
                      </v-layout>

                      <v-layout row justify-space-between align-center>
                        <v-flex class="text-truncate">
                          <v-tooltip top :disabled="false">
                            <!-- TODO: depend on length of caption - rough estimate but better than nothing -->
                            <v-layout row justify-left align-center slot="activator">
                              <v-icon small class="py-1 pl-0 pr-2">folder</v-icon>
                              <span class="text-truncate caption py-1">blahg ></span>
                            </v-layout>
                            <span>blahg > bread > crumb > blah > bread > crumb</span>
                          </v-tooltip>
                          <v-tooltip top>
                            <v-layout row justify-left align-center slot="activator">
                              <v-icon small class="py-1 pl-0 pr-2">cloud</v-icon>
                              <span class="text-truncate caption py-1">jossef.github.io</span>
                            </v-layout>
                            <span>https://jossef.github.io/material-design-icons-iconfont/?jossef.github.io/material-design-icons-iconfontjossef.github.io/material-design-icons-iconfont</span>
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
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>

    <v-footer app height="auto">
      <v-tooltip top>
        <strong slot="activator" class="subheading">DB name + 2 more</strong>
        <span>Existing descriptive text</span>
      </v-tooltip>

      <v-spacer></v-spacer>

      <v-btn class="mx-3" icon>
        <v-icon size="24px">settings</v-icon>
      </v-btn>
      <v-btn class="mx-3" icon>
        <v-icon size="24px">help</v-icon>
      </v-btn>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  computed: {
    darkTheme: () => window.matchMedia("prefers-color-scheme: dark").matches
  },
  data: () => ({
    show: true,
    dialog: false,
    drawer: true,
    team: "Philadelphia Eagles",
    items: [
      { icon: "contacts", text: "Team", route: "Team" },
      { icon: "event", text: "Schedule", route: "Schedule" },
      { icon: "account_balance", text: "Stadium", route: "Stadium" },
      { icon: "account_box", text: "Roster", route: "Roster" },
      {
        icon: "keyboard_arrow_up",
        "icon-alt": "keyboard_arrow_down",
        text: "Labels",
        model: true,
        children: [{ icon: "add", text: "Create label" }]
      },
      {
        icon: "keyboard_arrow_up",
        "icon-alt": "keyboard_arrow_down",
        text: "More",
        model: false,
        children: [
          { text: "Import" },
          { text: "Export" },
          { text: "Print" },
          { text: "Undo changes" },
          { text: "Other contacts" }
        ]
      },
      { icon: "settings", text: "Settings" },
      { icon: "chat_bubble", text: "Send feedback" },
      { icon: "help", text: "Help" },
      { icon: "phonelink", text: "App downloads" },
      { icon: "keyboard", text: "Go to the old version" }
    ]
  }),
  props: {
    source: String
  }
};
</script>
