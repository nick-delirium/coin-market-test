<template>
  <div class='block bg-grey move-up adaptive'>
    <div class="container">
      <div style='margin-top:-10em;width:100%'>
        <table class='table fixed_header' v-if='coins'>
          <thead>
            <tr>
              <th class='center laptop'>Rank</th>
              <th class='left tablefix'>Name</th>
              <th class='right'>Price</th>
              <th class='right laptop'>Market Cap</th>
              <th class='right laptop'>Volume (24h)</th>
              <th class='right'>Change (24h)</th>
            </tr>
          </thead>
          <tbody>
            <tr :class='{ "highlight": animateUpdated(coin) }' v-for='(coin, key) in shownCoins' :key='key'>
              <td class='center laptop'>{{coin.rank}}</td>
              <td class='left tablefix'>{{coin.name}} <br> {{coin.symbol}} </td>
              <td class='right'>{{parseSum(coin.priceUsd)}}</td>
              <td class='right laptop'>{{parseSum(coin.marketCapUsd)}}</td>
              <td class='right laptop'> {{parseSum(coin.volumeUsd24Hr)}} </td>
              <td class='right' :class='setColor(coin.changePercent24Hr)'>{{parsePerc(coin.changePercent24Hr)}}%</td>
            </tr>
            <div class='button' @click='page+=1'>Load More</div>
          </tbody>
        </table>
      </div>
    </div>
    
  </div>
</template>

<script>
export default {
  name: 'TableEl',
  data() {
    return {
      coins: null,
      page: 1,
      limit: 20
    };
  },
  computed: {
    shownCoins() {
      const end = this.page * this.limit;
      const start = 0;
      return this.coins.slice(start, end);
    }
  },
  beforeMount() {
    const self = this;
    this.$http
      .get('/assets?limit=2000')
      .then(r => {
        const { data } = r.data;
        this.coins = data;
      })
      .then(() => {
        const idArray = this.coins.map(item => item.id);

        const pricesWs = new WebSocket(
          `wss://ws.coincap.io/prices?assets=${idArray.join(',')}`
        );

        pricesWs.onmessage = (msg) => {
          const data = JSON.parse(msg.data);
          const updated = Object.keys(data);

          updated.map(item => {
            self.coins.find(x => x.id === item).priceUsd = data[item];
            self.coins.find(x => x.id === item).updated = true;
            setTimeout(() => {
              self.coins.find(x => x.id === item).updated = false;
            }, 300);
            return null;
          });
        };
      });
  },
  methods: {
    animateUpdated({ updated }) {
      return updated;
    },
    setColor(p) {
      return p > 0 ? 'green' : 'red';
    },
    parseSum(s) {
      const sum = parseFloat(s);

      const bill = 1000000000;
      const mill = 1000000;
      if (sum > bill) {
        return `$${(sum / bill).toFixed(2)}B`;
      } else if (sum < bill && sum > mill) {
        return `$${(sum / mill).toFixed(2)}m`;
      }
      return `$${sum.toFixed(2)}`;
    },
    parsePerc(p) {
      return parseFloat(p).toFixed(2);
    }
  }
};
</script>

<style lang='less'>
.adaptive {
  padding: 0 !important;
}
.table {
  width: 100%;
  background: #fff;
  margin: 1em 0;
  border: 1px solid rgba(34, 36, 38, 0.15);
  border-radius: 0.3rem;
  color: rgba(0, 0, 0, 0.87);
  border-collapse: separate;
  border-spacing: 0;
  font-size: 0.9em;
  box-shadow: rgba(0, 0, 0, 0.4) 0px 2px 15px -3px !important;
  th {
    background: #f9fafb;
    color: rgba(0, 0, 0, 0.87);
    padding: 0.92857143em 0.78571429em;
    vertical-align: inherit;
    font-weight: 700;
    text-transform: none;
    border-bottom: 1px solid rgba(34, 36, 38, 0.1);
    border-left: none;
    font-size: 0.9rem;
    font-weight: 500 !important;
    color: rgba(0, 0, 0, 0.6) !important;
    background: rgb(250, 250, 250) !important;
    border-left: none !important;
  }
  td {
    padding: 0.76em;
  }
  tbody tr td {
    border-top: 1px solid rgba(34, 36, 38, 0.1);
    font-weight: 500;
  }
  tbody tr td:not('.red') {
    color: black;
  }
  tbody tr:hover {
    transition: all 0.2s ease-in-out !important;
    background: rgba(0, 0, 0, 0.05) !important;
    color: rgba(0, 0, 0, 0.95) !important;
  }
}
.red {
  color: red;
}
.green {
  color: green;
}
.fixed_header {
  width: 100%;
  table-layout: fixed;
}

.fixed_header tbody {
  display: block;
  width: 100%;
  overflow: auto;
  height: 60vh;
}

.fixed_header thead tr {
  display: block;
}

.fixed_header th,
.fixed_header td {
  padding: 15px 15px;
  text-align: left;
  width: 200px;
}
.button {
  cursor: pointer;
  width: 100%;
  height: 60px;
  text-align: center;
  vertical-align: middle;
  line-height: 60px;
  color: white;
  background-color: rgb(24, 198, 131) !important;
}

@media only screen and (max-width: 768px) {
  .table {
    border-radius: 0;
  }
  .fixed_header tbody {
    display: block;
    width: 100%;
    overflow: auto;
    height: 74vh;
  }
  .laptop {
    display: none;
  }
  .left {
    width: 65vw;
  }
  .tablefix {
    width: 900px !important;
  }
}

@keyframes highlight {
  0% {
    background-color: transparent;
  }
  50% {
    background-color: rgba(237, 41, 57, 0.3);
  }
  100% {
    background-color: transparent;
  }
}

.highlight {
  animation-name: highlight;
  animation-duration: 0.8s;
}
</style>
