<template>
  <div id="searchPanel">
    <v-toolbar app class="py-2">
      <v-text-field
        solo
        :placeholder="$i18n('Search_label')"
        hide-details
        class="my-0"
        id="searchBox"
        name="cc5704978dc0411591addc66d25c325b"
        :value="currentSearchTerm"
        :title="$i18n('Search_tip')"
        @input="onSearchInput"
        autofocus
        @keyup.arrow-down.stop.prevent="focusFirstResult"
        @keyup.enter.stop.prevent="focusFirstResult"
        @keyup.escape.stop.prevent="handleEscape"
      ></v-text-field>
    </v-toolbar>

    <!-- TODO: list all matched logins here and hook up the text box to filter them too. Also must filter 
    search results to exclude those that are listed here so we avoid duplicates -->

    
    <Entry
      v-for="(entry, index) of filteredMatches"
      :key="entry.uniqueID"
      :entry="entry"
      :index="index"
    ></Entry>
    <!-- <Entry :show="true" :entry="entry"/>
    <Entry :show="false" :entry="entry"/>
    <Entry :show="true" :entry="entry"/> -->

<v-divider v-show="deduplicatedSearchResults && deduplicatedSearchResults.length > 0"></v-divider>
            <v-subheader
              v-show="deduplicatedSearchResults && deduplicatedSearchResults.length > 0"
              class="text-xs-center"
              style="justify-content: center;"
            >Matches from other sites</v-subheader>

    <!-- <div id="searchResults"> -->
    <!-- <v-list v-show="searchResults && searchResults.length > 0" id="searchResults-Container"> -->
    <Entry
      v-for="(entry, index) of deduplicatedSearchResults"
      :key="entry.uniqueID"
      :entry="entry"
      :index="index"
    ></Entry>
    <!-- </v-list> -->
    <!-- </div> -->
  </div>
</template>

<script lang="ts">
import { Component } from "vue";
import { mapState, mapActions, mapGetters, mapMutations } from "vuex";
import { names as actionNames } from "../../store/action-names";
import { SessionType } from "../../common/kfDataModel";
import { KeeState } from "../../store/KeeState";
import { ButtonAction } from "../../common/Button";
import { configManager } from "../../common/ConfigManager";
import { AddonMessage } from "../../common/AddonMessage";
import { Port } from "../../common/port";
import { Search, SearchResult, SearchOnlyMatches } from "../../common/search";
import Entry from "./Entry.vue";
import { mTypes } from "../../store";

export default {
  props: ['matchedLogins'],
  created(this: any) {
    this.onDBChanged = () => {
      this.search = new Search(
        {
          KeePassDatabases: this.$store.getters.KeePassDatabases,
          ActiveKeePassDatabaseIndex: this.$store.getters
            .ActiveKeePassDatabaseIndex
        } as any,
        {
          version: 1,
          searchAllDatabases: configManager.current.searchAllOpenDBs,
          maximumResults: 50
        }
      );
    };
    this.onDBChanged();
    this.searchOnlyMatches = new SearchOnlyMatches(this.matchedLogins);
    // this.filteredMatches = this.matchedLogins;
    this.searchOnlyMatches.execute(this.currentSearchTerm, (this as any).onSearchOnlyMatchesComplete.bind(this));
  },
  data() {
    return {
      filteredMatches: null,
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
  components: { Entry },

  mounted(this: any) {
    this.$store.subscribe((mutation, state) => {
      if (mutation.type === mTypes.updateKeePassDatabases) {
        this.onDBChanged();
      }
    });
  },
  computed: {
    ...mapGetters(["currentSearchTerm", "searchResults"]),
    deduplicatedSearchResults: function (this: any) {
      if (this.searchResults) {
        if (this.matchedLogins) {
          return this.searchResults.filter(
            e => !this.matchedLogins.some(m => m.uniqueID === e.uniqueID));
        } else {
          return this.searchResults;
        }
      }
      return null;
    }
  },

  methods: {
    ...mapActions(actionNames),
    onSearchOnlyMatchesComplete(logins: SearchResult[]) {
      console.log("onSearchOnlyMatchesComplete");
      logins = logins
        .sort(function(a, b) {
          if (a.relevanceScore > b.relevanceScore) return -1;
          if (a.relevanceScore < b.relevanceScore) return 1;
          return 0;
        });
      (this as any).filteredMatches = logins;
    },
    onSearchComplete(logins: SearchResult[]) {
      console.log("onSearchComplete");
      logins = logins
        .sort(function(a, b) {
          if (a.relevanceScore > b.relevanceScore) return -1;
          if (a.relevanceScore < b.relevanceScore) return 1;
          return 0;
        })
        .map(l => Object.assign(l, { fullDetails: null }));
      (this as any).$store.dispatch("updateSearchResults", logins);
      //(this as any).searchResults = logins;
    },
    onSearchInput(value) {
      const pm = (this as any).postMessage;
      pm({ currentSearchTerm: value } as AddonMessage);

      // Think this is OK but if it is actually async then user may have subsequent
      // characters deleted when the change is actually applied
      (this as any).$store.dispatch(
        "updateCurrentSearchTerm",
        value
      );
      (this as any).searchOnlyMatches.execute(
        value,
        (this as any).onSearchOnlyMatchesComplete.bind(this)
      );
      (this as any).search.execute(
        value,
        (this as any).onSearchComplete.bind(this),
        []
      );
    },
    focusFirstResult() {
      const filteredMatches = document.getElementById("filteredMatches-Container");
      if (filteredMatches && filteredMatches.firstElementChild) {
        (filteredMatches.firstElementChild as HTMLLIElement).focus();
        return;
      }
      const searchResults = document.getElementById("searchResults-Container");
      if (searchResults && searchResults.firstElementChild) {
        (searchResults.firstElementChild as HTMLLIElement).focus();
      }
    },
    handleEscape() {
      // This does not work in Firefox due to https://bugzilla.mozilla.org/show_bug.cgi?id=1373175
      const searchBox = document.getElementById(
        "searchBox"
      ) as HTMLInputElement;
      if (searchBox.value) {
        searchBox.value = "";
        searchBox.dispatchEvent(new Event("input"));
      } else {
        window.close();
      }
    }
  },
  mixins: [Port.mixin]
};
</script>
