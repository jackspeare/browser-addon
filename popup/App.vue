
<template>
  <v-app id="inspire" :dark="darkTheme">
    <v-content>
      <v-container fluid fill-height>
        <v-layout row wrap justify-center align-top>
          <v-flex xs12>

<!--               
                <div v-if="showNotifications" id="notifications">
                    <Notification v-for="n of notifications" :notification="n" :key="n.id"></Notification>
                </div>
                <SearchPanel v-show="showSearchPanel"></SearchPanel>

                
                <div class="list-group" id="menu-items">
                    <a v-show="showMatchedLogins" href="#" id="showMatchedLogins" class="list-group-item" @click="showMatchedLoginsPanel">{{ $i18n('matched_logins_label') }}</a>
                    <a v-show="showSaveLatestLogin" href="#" id="saveLatestLogin" class="list-group-item" @click="saveLatestLogin">{{ $i18n('saveLatestLogin') }}</a>
                    <a v-show="showGeneratePasswordLink" href="#" id="generatePasswordLink" class="list-group-item" @click="generatePassword">{{ $i18n('Menu_Button_copyNewPasswordToClipboard_label') }}</a>
                </div>

                 -->
            <v-text-field solo placeholder="Search..." hide-details class="mb-3"></v-text-field>

            <Entry :show="true" :entry="entry"/>
            <Entry :show="false" :entry="entry"/>
            <Entry :show="true" :entry="entry"/>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>

    <v-footer app height="auto">
      <v-tooltip top>
        <v-btn slot="activator" class="mx-0" icon id="password-open-kee-vault" @click="openKeeVault">
            <v-icon size="24px" color="blue">arrow_drop_down</v-icon>
        </v-btn>
        <span>{{ $i18n('Menu_Button_open_kee_vault_label') }}</span>
        </v-tooltip>
      <v-tooltip top>
        <v-btn slot="activator" class="mx-0" icon v-show="showOpenKeePassButton" id="password-open-keepass" @click="openKeePass">
            <v-icon size="24px" color="blue">lock_outline</v-icon>
        </v-btn>
        <span>{{ $i18n('Menu_Button_open_keepass_label') }}</span>
        </v-tooltip>
      <v-divider class="ml-1" vertical></v-divider>
      <v-icon size="20px" color="green" class="mx-2">lock</v-icon>
      <!-- TODO: red, orange or green based on connection status -->
      <v-tooltip top>
        <v-flex
          shrink
          slot="activator"
          class="caption py-1"
          style="word-break: break-word;overflow-wrap: break-word;"
        >DB name that moreDBnamethatgoesonandonforawhileandthensomemore</v-flex>
        <!-- TODO: "3 databases/vaults" -->
        <span>{{connectionStatus}}</span>
      </v-tooltip>

      <v-spacer></v-spacer>

<v-tooltip top>
        <v-btn slot="activator" class="mx-1" icon id="optionsLink" @click="showOptions">
            <v-icon size="24px">settings</v-icon>
        </v-btn>
        <span>{{ $i18n('Menu_Button_options_label') }}</span>
        </v-tooltip>
        <v-tooltip top>
        <v-btn slot="activator" class="mx-0" icon id="helpLink" @click="showHelp">
            <v-icon size="24px">help</v-icon>
        </v-btn>
        <span>{{ $i18n('Help_Centre_Button_label') }}</span>
        </v-tooltip>
    </v-footer>
  </v-app>
</template>

<script lang="ts">
import { Component } from "vue";
import Entry from "./components/Entry.vue";
import { mapState, mapActions, mapGetters, mapMutations } from 'vuex';
import { names as actionNames } from '../store/action-names';
import { SessionType } from '../common/kfDataModel';
import { KeeState } from '../store/KeeState';
import Notification from "./Notification.vue";
import SearchPanel from "./SearchPanel.vue";
import { Port } from '../common/port';
import { Action } from '../common/Action';
import { KeeLog } from '../common/Logger';

export default {
  data() {
    return {
      show: true,
      entry: {
        title: "Title sdfkjhsdfkljdf s dfl;kjsdhfsdfjdjhksdfh dsfdsfsdfgsdfg",
        usernameValue: "Username.emailaddress@emailaddress.com.longish",
        groupPath: "blahg > bread > crumb > blah > bread > crumb",
        URL:
          "https://jossef.github.io/material-design-icons-iconfont/?jossef.github.io/material-design-icons-iconfontjossef.github.io/material-design-icons-iconfont",
        domain: "jossef.github.io",
        iconURI:
          "url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAgY0hSTQAAeiYAAICEAAD6AAAAgOgAAHUwAADqYAAAOpgAABdwnLpRPAAAAgxJREFUOI2lkk9Ik2Ecxz/P8+7Pu9xKmRlNmYIgZSpGVhiO1qFDdOgWQVAEZnQzDx2mhyAN8tIluhd08RYUhLJhQUU1KYiiS7mgckJL073b3N73fTq8S13bIel7ep4fz/f5fb+/708AMDydRIoDbBW2PSC48vg1QuvbMrkM+T9kAFfNqgKXrnGmu4l6n4tHHzPMLxogRQ0FVWTF1Pke0qMD7Kxzs5QzuXu6k8x4FOmufl6pwFbcO9vFwsoajWOz6x3vJxdo31VHbjyKfjUOYkOJRv+5awBdoQBvRw6zb7ef6O1kpVwBS0YJAfS2BHj55df6J4KRGdXTHCBxaT+hiecU8wX6PPMkzQ5nGJvlejVKN47xI1uk+9Yr0ssFJEoxPdhLaOIZZjHH5+BlpgKTXPAmqPesEGt7AMrxbhYsxPAMbTdf8H1sAAS4sKFpu5di3gTpw0YgULyxW/kWGQIEOUvn6dpBrh96x8mHRzFyJSYTKY53BJFIwYd0lpbGbRVyDcuDqZwZJ7Nh4qfiRJoXudj5CYBwg87PvIlEQP+dOVKxI+wJ+f9y7aBkSyzlDO2r4WOov4UTe4PMpZadPVg1SuixWTyapKB0LCStWgbD8mEpjXY9w2rRja0EYX+O8A4vDaNPQAonhcp+0skNVQ6hfP5zF4DYoLhQtkJsDt2uyL8C1ZuMRBGpYfvfoNT734y1s7OXU2CAAAAAAElFTkSuQmCC')",
        fields: [
          {
            value: "Username.emailaddress@emailaddress.com.longish",
            name: "username",
            type: "text"
          },
          {
            value: "secret 1",
            name: "password",
            type: "password"
          },
          {
            value: "secret 2",
            name: "another password",
            type: "password"
          }
        ]
      }
    };
  },

  computed: {
    ...mapGetters(['showGeneratePasswordLink', 'showSaveLatestLogin', 
    'showMatchedLogins','showOpenKeePassButton','connectionStatus', 
    'notifications', 'showNotifications', 'showSearchPanel']),
    darkTheme: () => window.matchMedia("prefers-color-scheme: dark").matches    
  },

  methods: {
    ...mapActions(actionNames),
    showOptions: () => {
        browser.runtime.openOptionsPage();
        window.close();
    },
    generatePassword: () => {
        Port.postMessage({ action: Action.GeneratePassword });
        window.close();
    },
    saveLatestLogin: () => {
        Port.postMessage({ action: Action.SaveLatestLogin });
        window.close();
    },
    showMatchedLoginsPanel: () => {
        Port.postMessage({ action: Action.ShowMatchedLoginsPanel });
        window.close();
    },
    showHelp: () => {
        browser.tabs.create({ url: "https://www.kee.pm/help" });
        window.close();
    },
    openKeeVault: async function() {
        KeeLog.debug("open Kee Vault requested");
        const vaultTabs = await browser.tabs.query({url: ["https://keevault.pm/*", "https://app-beta.kee.pm/*", "https://app-dev.kee.pm/*"]});
        if (vaultTabs && vaultTabs[0]) {
            browser.tabs.update(vaultTabs[0].id, { active: true });
        } else {
            browser.tabs.create({
                url: "https://keevault.pm/",
                active: true
            });
        }
        window.close();
    },
    openKeePass: function() {
        KeeLog.debug("open KeePass requested");
        if ((this as any).$store.state.connectedWebsocket) {
            Port.postMessage({ action: Action.OpenKeePass });
        } else {
            KeeLog.info("KeePass no longer connected so taking no action");
        }
        window.close();
    }
  },

  components: {
      Notification,
      SearchPanel,
      Entry
  },

  mounted: function () {
    // Hack around Firefox bug https://bugzilla.mozilla.org/show_bug.cgi?id=1516132
    // and https://bugzilla.mozilla.org/show_bug.cgi?format=default&id=1416505
    document.addEventListener("DOMContentLoaded", () => {
        let count = 0;
        const timer = setInterval(() => {
            let newPl = "";
            const pl = document.getElementById("searchBox").getAttribute("placeholder");
            if (pl.endsWith(" ")) newPl = pl.trim();
            else newPl = pl + " ";
            document.getElementById("searchBox").setAttribute("placeholder", newPl);
            count++;
            if (count > 20) clearInterval(timer);
        }, 200);
    });
  },
  mixins: [Port.mixin]
};
</script>
