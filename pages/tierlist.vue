<template>
  <div>
    <div v-for="champion in championData" :key="champion.championId">
      <div class="card" style="width: 23vw; height: 350px; float: left">
        <div class="card-content">
          <div class="media">
            <div class="media-left">
              <figure class="image is-48x48">
                <img src="~/assets/champion/Ahri.png" alt="Placeholder image" />
              </figure>
            </div>
            <div class="media-content">
              <p class="title is-4">{{champion.name}}</p>
              <p class="subtitle is-6">{{champion.tags}}</p>
            </div>
          </div>

          <div class="content">
            {{champion.description}}
            <NuxtLink
              :to="`championDetails/${champion.championId}`"
              exact-active-class="is-active"
            >Details</NuxtLink>
            <br />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { data } from "browserslist";
import gql from "graphql-tag";

const CHAMPION_DATA = gql`
  query SUMMONER_BY_USERNAME($sorted: String!) {
    championData(sorted: $sorted) {
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
  apollo: {
    championByAlphabetData: {
      query: CHAMPION_DATA,
      manual: true,
      variables: {
        sorted: "looseratio",
      },
      result(data) {
        if (data.data !== null) {
            this.championData= [];
          for (let i = 0; i < data.data.championData.length; i++) {
            this.championData.push({
              name: data.data.championData[i].name,
              title: data.data.championData[i].title,
              description: data.data.championData[i].description,
              tags: data.data.championData[i].tags,
              winCount: data.data.championData[i].winCount,
              looseCount: data.data.championData[i].looseCount,
              championId: data.data.championData[i].championId,
              damageDealtToBuildings: data.data.championData[i].damageDealtToBuildings,
              damageDealtToObjectives:
                data.data.championData[i].damageDealtToObjectives,
              damageSelfMitigated: data.data.championData[i].damageSelfMitigated,
              topCount: data.data.championData[i].topCount * 100 + "%",
              midCount: data.data.championData[i].midCount * 100 + "%",
              jungleCount: data.data.championData[i].jungleCount * 100 + "%",
              adcCount: data.data.championData[i].adcCount * 100 + "%",
              supportCount: data.data.championData[i].supportCount * 100 + "%",
              visionScore: data.data.championData[i].visionScore,
              wardsPlaced: data.data.championData[i].wardsPlaced,
              totalMinionsKilled: data.data.championData[i].totalMinionsKilled,
              totalHeal: data.data.championData[i].totalHeal,
              totalDamageTaken: data.data.championData[i].totalDamageTaken,
              totalDamageDealtToChampions:
                data.data.championData[i].totalDamageDealtToChampions,
              goldEarned: data.data.championData[i].goldEarned,
              kills: data.data.championData[i].kills,
              deaths: data.data.championData[i].deaths,
              assists: data.data.championData[i].assists,
              summonerLevel: data.data.championData[i].summonerLevel,
              totalTimeSpendDead: data.data.championData[i].totalTimeSpendDead,
              count: data.data.championData[i].count,
              img: "/assets/champion/" + data.data.championData[i].name +".png"
            });
          }
          console.log(this.championData[0].img)
        }
      },
    },
  },
  name: "Tierlist",
  data() {
    return {
      championData: [
        {
          name: "Aatrox",
          title: "Die Klinge der Düsteren",
          description:
            "Einst waren Aatrox und seine Brüder ehrenhafte Verteidiger Shurimas gegen die Leere, die zu einer noch größeren Bedrohung für Runeterra wurden und schließlich nur durch hinterlistige, sterbliche Zauberei besiegt werden konnten. Aber nach Jahrhunderten...",
          tags: "Fighter|Tank",
          winCount: 0.47,
          looseCount: 0.53,
          championId: "266",
          damageDealtToBuildings: 2896.93,
          damageDealtToObjectives: 7017.78,
          damageSelfMitigated: 29745.08,
          topCount: 0.76,
          midCount: 0.03,
          jungleCount: 0.01,
          adcCount: 0,
          supportCount: 0,
          visionScore: 14.32,
          wardsPlaced: 6.93,
          totalMinionsKilled: 132.94,
          totalHeal: 10319.57,
          totalDamageTaken: 30669.88,
          totalDamageDealtToChampions: 18380.62,
          goldEarned: 11332.55,
          kills: 6.13,
          deaths: 6.74,
          assists: 6.33,
          summonerLevel: 281.48,
          totalTimeSpendDead: 203.08,
          count: 2477,
        },
      ],
    };
  },
};
</script>
<style>
</style>