<template>
  <div class="main-info">
    <div class="mobileView">
      <div class="spoiler">
        Market snapshot <img src="../assets/drop.png" width='12px' :style='rotate' @click='spoilerOpen = !spoilerOpen' alt="">
      </div>
      <div :style='mobileDisplay' class="data">
          <StatCell :label="'Market Cap'" :value="markets.cap" />
          <StatCell :label="'Exchange Vol'" :value="markets.exchangesCap" />
          <StatCell :label="'Assets'" :value="markets.assetsNum" />
          <StatCell :label="'Exchanges'" :value="markets.exchangesNum" />
          <StatCell :label="'Markets'" :value="markets.marketsNum" />
          <StatCell :label="'BTC DOM Index'" :value="markets.btcIndex" />
      </div>
    </div>
    <div class="container col-row center">
      <div class="row">
        <div class="square">
          <StatCell :label="'market cap'" :value="markets.cap" />
        </div>
        <div class="square">
          <StatCell :label="'exchange vol'" :value="markets.exchangesCap" />
        </div>
        <div class="square">
          <StatCell :label="'assets'" :value="markets.assetsNum" />
        </div>
      </div>
      <div class="row">
        <div class="square">
          <StatCell :label="'exchanges'" :value="markets.exchangesNum" />
        </div>
        <div class="square">
          <StatCell :label="'markets'" :value="markets.marketsNum" />
        </div>
        <div class="square">
          <StatCell :label="'BTC DOM INDEX'" :value="markets.btcIndex" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import StatCell from './StatCell.vue'

export default {
  name: 'main-info',
  components: { StatCell },
  data() {
    return {
      spoilerOpen: false,
      markets: {
        cap: null,
        assetsNum: null,
        marketsNum: null,
        marketNames: null,
        exchangesNum: null,
        exchangesCap: null,
        btcIndex: '53%',
      }
    }
  },
  computed: {
    rotate() {
      return {
        "padding": "5px",
        "float": "right",
        "transform": !this.spoilerOpen ? "rotate(90deg)" : "none"
      }
    },
    mobileDisplay() {
      return {
        "display": this.spoilerOpen ? "block" : "none"
      }
    }
  },
  beforeMount() {
    const self = this;
    this.$http.get('/assets?limit=2000')
    .then(r => {
      const { data } = r.data;
      const wholeCap = data.filter(item => item.marketCapUsd != null)
        .map((item) => parseFloat(item.marketCapUsd))
        .reduce((a,b) => a+b)
      this.markets.cap = this.formatCap(wholeCap)
      this.markets.assetsNum = data.length
    })
    this.$http.get('/exchanges?limit=2000')
    .then(r => {
      const { data } = r.data;
      const exchCap = data.filter(item => item.volumeUsd != null)
        .map(item => parseFloat(item.volumeUsd))
        .reduce((a, b) => a+b);
      this.markets.exchangesNum = data.length;
      this.marketNames = r.data.id
      this.markets.exchangesCap = this.formatCap(exchCap);
    })
    .then(() => {
      const tradeWs = new WebSocket('wss://ws.coincap.io/trades/binance')

        tradeWs.onmessage = (msg) => {
          const data = JSON.parse(msg.data);
          const updated = Object.keys(data);

          updated.map(item => {
            self.exchangesCap += data[item];
            return null;
          });
        };
    })
    this.$http.get('/markets?limit=2000', {
      limit: 2000
    })
    .then(r => {
      const { data } = r.data;
      this.markets.marketsNum = data.length
    })
  },
  methods: {
    formatCap(item) {
      return `$${(item/1000000000).toFixed(2)}B`;
    }
  }
}
</script>

<style>

.data .statistics:first-child .mobile  {
  margin-top:5px
}
.data .statistics:first-child .mobile hr {
  display: none!important
}
.row {
  -webkit-flex: 5;
  -moz-flex: 5;
  -ms-flex: 5;
  -o-flex: 5;
  flex: 5;
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-flex-direction: row;
  -moz-flex-direction: row;
  -ms-flex-direction: row;
  -o-flex-direction: row;
  flex-direction: row;
  margin-bottom: 20px;
}

.square {
  color: white;
  -webkit-flex: 4;
  -moz-flex: 4;
  -ms-flex: 4;
  -o-flex: 4;
  flex: 4;
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  text-align: center;
  -webkit-justify-content: center;
  -moz-justify-content: center;
  -ms-justify-content: center;
  -o-justify-content: center;
  justify-content: center;
  -webkit-flex-flow: column wrap;
  -moz-flex-flow: column wrap;
  -ms-flex-flow: column wrap;
  -o-flex-flow: column wrap;
  flex-flow: column wrap;
}
.col-row {
  margin-top: 25px;
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -o-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-flex-direction: column;
  -moz-flex-direction: column;
  -ms-flex-direction: column;
  -o-flex-direction: column;
  flex-direction: column;
}
.main-info {
  margin: 0 -1px;
  padding: 1.5em;
  position: relative;
  display: block;
  padding-bottom: 10em !important;
  background: linear-gradient(to right, rgb(63, 81, 181), rgb(100, 181, 246)) rgb(255, 255, 255) !important;
  border-width: initial !important;
  border-style: none !important;
  border-color: initial !important;
  border-image: initial !important;
}
.mobileView {
  display: none
}
.spoiler {
  color: white
}
@media only screen and (max-width: 768px) {
  .mobileView {
    display: block;
  }
  .col-row {
    display: none;
  }
  .data {
    display: none;
  }
}
</style>
