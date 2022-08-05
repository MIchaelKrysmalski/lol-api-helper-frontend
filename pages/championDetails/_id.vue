<template>
  <div class="card">
    <div class="card-content">
      <div class="media">
        <div class="media-left">
          <figure class="image is-48x48">
            <img src="~assets/champion/Ahri.png" alt="Placeholder image" />
          </figure>
        </div>
        <div class="media-content">
          <p class="title is-4">{{championData.name}} ({{championData.championId}})</p>
          <p class="subtitle is-6">{{championData.tags}}</p>
        </div>
      </div>

      <div class="content">
        <p>Analysed DataCount: {{championData.count}}</p>
        <p>Winratio: {{championData.winCount * 100}}%</p>
        <p>TopCount: {{championData.topCount * 100}}%</p>
        <p>MidCount: {{championData.midCount * 100}}%</p>
        <p>JungleCount: {{championData.jungleCount * 100}}%</p>
        <p>BottomCount: {{championData.adcCount * 100}}%</p>
        <p>SupportCount: {{championData.supportCount * 100}}%</p>
        <p>VisionScore: {{championData.visionScore}}</p>
        <p>WardsPlaced: {{championData.wardsPlaced}}</p>
        <p>TotalMinionsKilled: {{championData.totalMinionsKilled}}</p>
        <p>Total Heal: {{championData.totalHeal}}</p>
        <p>Total Damage Taken: {{championData.totalDamageTaken}}</p>
        <p>Total Damage Dealt to Champions: {{championData.totalDamageDealtToChampions}}</p>
        <p>Gold Earned: {{championData.goldEarned}}</p>
        <p>Kills: {{championData.kills}}</p>
        <p>Deaths: {{championData.deaths}}</p>
        <p>Assists: {{championData.assists}}</p>
        <p>summonerLevel: {{championData.summonerLevel}}</p>
        <p>totalTimeSpendDead: {{championData.totalTimeSpendDead}}</p>
        <br />

      </div>
    </div>
  </div>
</template>
<script>
import gql from 'graphql-tag'
const CHAMPION_DATA_BY_ID = gql`
  query CHAMPIONDATA_BY_ID($id: String!) {
    getChamoionDataById(id: $id) {
      name
      title
      description
      tags
      winCount
      looseCount
      championId
      damageDealtToBuildings
      damageDealtToObjectives
      damageSelfMitigated
      topCount
      midCount
      jungleCount
      adcCount
      supportCount
      visionScore
      wardsPlaced
      totalMinionsKilled
      totalHeal
      totalDamageTaken
      totalDamageDealtToChampions
      goldEarned
      kills
      deaths
      assists
      summonerLevel
      totalTimeSpendDead
      count
    }
  }
`;
export default {
  name: "ChampionDetails",
  apollo: {
    championDataById: {
        query: CHAMPION_DATA_BY_ID,
        manual: true,
        variables: {
            id: "103"
        },
        result(data){
            if(data.data){
                this.championData = data.data.getChamoionDataById
                console.log(this.championData);
            }
        }
    }
  },
  methods: {
    getChampionData(){
        this.$apollo.queries.championDataById.refetch({
            id: this.$route.params.id.toString(),
        })
    }
  },
  data() {
    return {
        championData:{},
        
    }
  },
  beforeMount(){
    this.getChampionData();
  }
};
</script>
<style scoped>
</style>