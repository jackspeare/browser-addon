
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
                
                <div class="list-group" id="menu-items">
                    <a v-show="showMatchedLogins" href="#" id="showMatchedLogins" class="list-group-item" @click="showMatchedLoginsPanel">{{ $i18n('matched_logins_label') }}</a>
                    <a v-show="showSaveLatestLogin" href="#" id="saveLatestLogin" class="list-group-item" @click="saveLatestLogin">{{ $i18n('saveLatestLogin') }}</a>
                    <a v-show="showGeneratePasswordLink" href="#" id="generatePasswordLink" class="list-group-item" @click="generatePassword">{{ $i18n('Menu_Button_copyNewPasswordToClipboard_label') }}</a>
                </div>

                 -->
            

<SearchPanel v-show="showSearchPanel" :matchedLogins="matchedLogins"></SearchPanel>

          </v-flex>
        </v-layout>
      </v-container>
    </v-content>

    <v-footer app height="auto">
      <v-tooltip top>
        <v-btn :aria-label="$i18n('Menu_Button_open_kee_vault_label')" slot="activator" class="mx-0" icon id="password-open-kee-vault" @click="openKeeVault">
            <img width="24px" height="24px" src="../common/images/48-kee-vault.png" />
        </v-btn>
        <span>{{ $i18n('Menu_Button_open_kee_vault_label') }}</span>
        </v-tooltip>
      <v-tooltip top>
        <v-btn :aria-label="$i18n('Menu_Button_open_keepass_label')" slot="activator" class="mx-0" icon v-show="showOpenKeePassButton" id="password-open-keepass" @click="openKeePass">
            <img width="24px" height="24px" src="../common/images/48-keepass.png" />
        </v-btn>
        <span>{{ $i18n('Menu_Button_open_keepass_label') }}</span>
        </v-tooltip>
      <v-divider class="ml-1" vertical></v-divider>
      <v-icon size="20px" :color="statusIconColour" class="mx-2">lock</v-icon>
      <v-tooltip top>
        <v-flex
          shrink
          slot="activator"
          class="caption py-1"
          style="word-break: break-word;overflow-wrap: break-word;"
        >{{connectionStatus}}</v-flex>
        <span>{{connectionStatusDetail}}</span>
      </v-tooltip>

      <v-spacer></v-spacer>

<v-tooltip top>
        <v-btn :aria-label="$i18n('Menu_Button_options_label')" slot="activator" class="mx-1" icon id="optionsLink" @click="showOptions">
            <v-icon size="24px">settings</v-icon>
        </v-btn>
        <span>{{ $i18n('Menu_Button_options_label') }}</span>
        </v-tooltip>
        <v-tooltip top>
        <v-btn :aria-label="$i18n('Help_Centre_Button_label')" slot="activator" class="mx-0" icon id="helpLink" @click="showHelp">
            <v-icon size="24px">help</v-icon>
        </v-btn>
        <span>{{ $i18n('Help_Centre_Button_label') }}</span>
        </v-tooltip>
    </v-footer>
  </v-app>
</template>

<script lang="ts">
import { Component } from "vue";
import { mapState, mapActions, mapGetters, mapMutations } from 'vuex';
import { names as actionNames } from '../store/action-names';
import { SessionType } from '../common/kfDataModel';
import { KeeState } from '../store/KeeState';
import Notification from "./components/Notification.vue";
import SearchPanel from "./components/SearchPanel.vue";
import { Port } from '../common/port';
import { Action } from '../common/Action';
import { KeeLog } from '../common/Logger';

export default {
    props: ['matchedLogins'],
  computed: {
    ...mapGetters(['showGeneratePasswordLink', 'showSaveLatestLogin', 
    'showMatchedLogins','showOpenKeePassButton','connectionStatus',
    'connectionStatusDetail', 'connected', 'databaseIsOpen',
    'notifications', 'showNotifications', 'showSearchPanel']),
    darkTheme: () => window.matchMedia("prefers-color-scheme: dark").matches,
    statusIconColour: function (this: any) {
        if (this.connected && this.databaseIsOpen) {
            return "green";
        } else if (this.connected) {
            return "orange";
        }
        return "red";
    }
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
      SearchPanel
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
