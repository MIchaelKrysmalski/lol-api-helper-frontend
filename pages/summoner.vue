<template>
  <div>
    <div class="columns">
      <div class="column" style="margin: 10px auto">
        <b-field message="Enter Username">
          <b-input
            placeholder="Username"
            type="search"
            icon="magnify"
            v-model="username"
          >
          </b-input>
          <p class="control">
            <b-button
              type="is-primary"
              label="Search"
              @click="searchSummoner"
            />
          </p>
        </b-field>
      </div>
      <div class="column" style="margin: 10px auto">
        <b-button type="is-danger" label="Clear" @click="clearSummoner" />
      </div>
    </div>
    <div v-if="foundUser">
    <div class="columns">
         <b-collapse class="card" animation="slide" aria-id="contentIdForA11y3">
          <template #trigger="props">
            <div
              class="card-header"
              role="button"
              aria-controls="contentIdForA11y3"
              :aria-expanded="props.open"
            >
              <p class="card-header-title">player Details</p>
              <a class="card-header-icon">
                <b-icon :icon="props.open ? 'menu-down' : 'menu-up'"> </b-icon>
              </a>
            </div>
          </template>
          <div class="card">
  <div class="card-image">
    <figure class="image is-4by3">
      <img src="https://bulma.io/images/placeholders/1280x960.png" alt="Placeholder image">
    </figure>
  </div>
  <div class="card-content">
    <div class="media">
      <div class="media-left">
        <figure class="image is-48x48">
          <img :src="profileIcon" alt="Placeholder image">
        </figure>
      </div>
      <div class="media-content">
        <p class="title is-4">{{playerData.name}}</p>
        <p class="subtitle is-6">{{playerData.id}}</p>
      </div>
    </div>

    <div class="content">
      <p>Last time played: {{new Date(playerData.revisionDate)}}</p>
      <p>Solo:</p>
      <p>Rank: {{playerData.soloTier}} {{playerData.soloRank}}</p>
      <p>Wins: {{playerData.soloWins}}</p>
      <p>Looses: {{playerData.soloWins}}</p>
      <p>Leaguepoints: {{playerData.soloWins}}</p>
      <br>
      <p>Flex:</p>
      <p>Rank: {{playerData.flexTier}} {{playerData.flexRank}}</p>
      <p>Wins: {{playerData.flexWins}}</p>
      <p>Looses: {{playerData.flexWins}}</p>
      <p>Leaguepoints: {{playerData.flexWins}}</p>
    </div>
  </div>
</div>
        </b-collapse>
    </div>
      <div class="columns">
        <b-collapse class="card" animation="slide" aria-id="contentIdForA11y3">
          <template #trigger="props">
            <div
              class="card-header"
              role="button"
              aria-controls="contentIdForA11y3"
              :aria-expanded="props.open"
            >
              <p class="card-header-title">Champion Masteries</p>
              <a class="card-header-icon">
                <b-icon :icon="props.open ? 'menu-down' : 'menu-up'"> </b-icon>
              </a>
            </div>
          </template> 
          <MasteryTable
            :championMastery="championMasteries"
          ></MasteryTable>
        </b-collapse>
      </div>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag";
import MasteryTable from "../components/MasteryTable.vue";

const SEARCH_SUMMONER_QUERY = gql`
  query SUMMONER_BY_USERNAME($name: String!) {
    SummonerData(name: $name) {
      id
      name
      accountId
      puuid
      profileIconId
      revisionDate
      summonerLevel
      championMastery {
        id
        name
        level
        points
        lastPlayTime
        chestGranted
      }
      soloTier
      soloRank
      soloWins
      soloLosses
      soloLeaguePoints
      flexTier
      flexRank
      flexWins
      flexLosses
      flexLeaguePoints
    }
  }
`;
export default {
  apollo: {
    summonerData: {
      query: SEARCH_SUMMONER_QUERY,
      manual: true,
      variables: {
        name: "",
      },
      result(data) {
        if (data.data !== null) {
          this.playerData = data.data.SummonerData;
          if (this.playerData.id !== "") {
            this.foundUser = true;
            this.championMasteries = [];
            for (let i = 0; i < this.playerData.championMastery.length; i++) {
                let date = new Date(this.playerData.championMastery[i].lastPlayTime);
              this.championMasteries.push({
                id: this.playerData.championMastery[i].id,
                name: this.playerData.championMastery[i].name,
                level: this.playerData.championMastery[i].level,
                points: this.playerData.championMastery[i].points,
                lastPlayTime: (date.getDay() + 1) + "." + (date.getMonth() + 1) +"." + date.getFullYear(),
                chestGranted: this.playerData.championMastery[i].chestGranted,
              });
            }
            this.profileIcon = "https://ddragon.leagueoflegends.com/cdn/12.6.1/img/profileicon/"+1+".png";
          }
        }
      },
    },
  },
  name: "Summoner",
  data() {
    return {
      username: "",
      foundUser: false,
      playerData: {},
      championMasteries: [],
      profileIcon: ""
    };
  },
  components: {
    MasteryTable,
  },
  methods: {
    async searchSummoner() {
      if (this.username) {
        this.loadasyncData();
      }
    },
    clearSummoner() {
      this.foundUser = false;
      this.playerData = {};
      this.username = "";
    },
    async loadasyncData() {
      this.$apollo.queries.summonerData.refetch({
        name: this.username,
      });
    },
  },
};
</script>
<style lang="css">
</style>