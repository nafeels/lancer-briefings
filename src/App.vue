<template>
  <Header :header="this.header" />
  <div class="content-container">
    <section class="section-container" id="missions" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/mission-icon.svg" />
        <h1>Relatório de Missões</h1>
      </div>
      <div class="section-content-container">
        <h3>Atribuições Atuais</h3>
        <Markdown :source="current_md" class="markdown" />
        <h3>Quadro de Missões</h3>
        <div class="mission-list-container">
          <Mission
            v-for="item in this.missions"
            :key="item.slug"
            :mission="item"
            :selected="this.mission_slug"
            @click="selectMission(item)"
          />
        </div>
      </div>
    </section>
    <section class="section-container" id="events" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/events-icon.svg" />
        <h1>Relatório de Eventos</h1>
      </div>
      <div class="section-content-container">
        <Markdown :source="events" class="markdown" />
      </div>
    </section>
    <section class="section-container" id="pilots" style="width:894px; height:714px;">
      <div style="height:52px; overflow:hidden;">
        <div class="section-header clipped-medium-backward-pilot">
          <img src="/icons/pilot-icon.svg" />
          <h1>Registro de Pilotos</h1>
        </div>
        <div class="rhombus-back">&nbsp;</div>
      </div>
      <div class="section-content-container">
        <div class="pilot-list-container">
          <Pilot v-for="item in this.pilots" :key="item.slug" :pilot="item" />
        </div>
      </div>
    </section>
  </div>
  <svg
    style="visibility: hidden; position: absolute;"
    width="0"
    height="0"
    xmlns="http://www.w3.org/2000/svg"
    version="1.1"
  >
    <defs>
      <filter id="round">
        <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />
        <feColorMatrix
          in="blur"
          mode="matrix"
          values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -5"
          result="goo"
        />
        <feComposite in="SourceGraphic" in2="goo" operator="atop" />
      </filter>
    </defs>
  </svg>
  <audio autoplay>
    <source src="/startup.ogg" type="audio/ogg" />
  </audio>
  <Footer/>
</template>

<script>
import Header from './components/layout/Header.vue';
import Footer from './components/layout/Footer.vue';
import Mission from './components/Mission.vue';
import Pilot from './components/Pilot.vue';
import Markdown from 'vue3-markdown-it';

export default {
  components: {
    Header,
    Footer,
    Mission,
    Pilot,
    Markdown
  },

  data() {
    return {
      "mission_slug": "000",
      "current_md": "",
      "events": "",
      "missions": [
        {
          "slug": "001",
          "name": "A Descida",
          "status": "start"
        },
      ],
      "pilots": [
        {
          "callsign": "Scypher",
          "alias": 'Nova Forge',
          "code": "8fe0bc2f-7bd7-402f-b66c-894d32c1d06c//NDL-C-SOLAR-CROSS//BIOMETRIC RECORD VALID [[15.43PB]] :: OHM C//5025-03-15T19:12:31.48",
          "corpro": "SMG",
          "frame": "Cholomungma",
          "mech": "Sem Dados"
        },
        {
          "callsign": "Phantom",
          "alias": "Vladimir Kalashnikov",
          "code": "2067d2d7-ca1f-4bc7-8464-b2d74b1d3864//NDL-C-IRON-HINDSIGHT//BIOMETRIC RECORD VALID [[14.81PB]] :: OHM C//5025-03-15T19:15:31.183Z",
          "corpro": "SMG",
          "frame": "Evereste",
          "mech": "GHOST REAPER"
        },
        {
          "callsign": "Ynx",
          "alias": "Aurora Galanis",
          "code": "30f58c3d-3416-43a3-8102-8d2235fa9bb1//NDL-C-LUNAR-OCTOBER//BIOMETRIC RECORD VALID [[16.56PB]] :: OHM C//5025-03-15T19:14:47.683Z",
          "corpro": "SMG",
          "frame": "Evereste",
          "mech": "Bola de Neve"
        },
        {
          "callsign": "Zaytoun",
          "alias": "Kian Samud Olvire",
          "code": "eedd65b9-a314-4675-8e5d-9ed09a0474d4//NDL-C-NULL-WILD//BIOMETRIC RECORD VALID [[15.2PB]] :: OHM C//5025-03-15T19:13:30.577Z",
          "corpro": "SMG",
          "frame": "Sagarmatha",
          "mech": "Terrafirme"
        },
        {
          "callsign": "Halo",
          "alias": "Paul Petrisco",
          "code": "64120932-6575-405c-b56e-3befcb3ad1ed//NDL-C-EARNEST-ORRERY//BIOMETRIC RECORD VALID [[13.95PB]] :: OHM C//5025-03-15T19:14:00.334Z",
          "corpro": "SMG",
          "frame": "Evereste",
          "mech": "Sem Dados"
        },
        {
          "callsign": "Sem Dados",
          "alias": 'Sem Dados',
          "code": "d1fdf62e-d81e-4e10-97c8-df3bc4860117///NDL-C-DEEP-STATION//5a4254aa-9fa2-42ca-a077-8f5bfd1e1ad3",
          "corpro": "N/A",
          "frame": "N/A",
          "mech": "Sem Dados"
        },
      ],
      "header": {
        "planet": "Cressidium",
        "year": "5016u",
        "system": "Sem Dados",
        "gate": "Sem Dados",
        "ring": "Linha Cascades",
        "headerTitle": "União",
        "headerSubtitle": "Departamento da Marinha",
        "subheaderTitle": "Time de Segurança e Defesa",
        "subheaderSubtitle": "ORION",
      },
      "options":{
        "eventsMarkdownPerMission": true
      }
    }
  },

  created() {
    this.loadMissionMarkdown()
    this.loadEventsMarkdown()
  },

  computed: {

  },

  methods: {
    selectMission(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown()
      if(this.options.eventsMarkdownPerMission){
        this.loadEventsMarkdown();
      }
    },
    loadMissionMarkdown() {
      let self = this;
      let md = `/missions/${self.mission_slug}.md`
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.current_md = client.responseText;
      }
      client.send();
    },
    loadEventsMarkdown() {
      let self = this;
      let md = "";

      if(self.options.eventsMarkdownPerMission){
        md = `/events/${self.mission_slug}.md`
      }
      else {
        md = "/events.md"
      }

      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.events = client.responseText;
      }
      client.send();
    }
  }

}
</script>


<style lang="scss">
#app {
  width: 1902px;
  height: 910px;
  overflow: hidden;
}
</style>
