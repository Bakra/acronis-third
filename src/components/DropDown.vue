<template>
  <div>
    <div>
      <button @click='toggleShow' class='anchor'>{{activeOption}}</button>
      <div v-if='showMenu' class='menu'>
        <div class='menu-item' :key="index" v-for='(item,index) in options' @click='itemClicked(item)'>{{item.abbr}}</div>
      </div>
    </div>
    <div>
      <button class='search' @click='onSearch'>Search</button>
    </div>
    <a :href='`${url}`' target="_blank">{{url}}</a>
</div>
</template>

<script>
export default {
  name: 'DropDown',
  data: function() {
    return {
      showMenu: false,
      activeOption: 'Selent an option',
      url: '',
      options: []
    }
  },
  mounted: function() {
    this.axios.get('https://cors-anywhere.herokuapp.com/https://www.acronis.com/en-us/api/v1/price/?machine_name=acronis_backup&locale=en-us').then((response) => {
      this.options = response.data[0].subscription.buy
    })
  },
  methods: {
    toggleShow: function() {
      this.showMenu = !this.showMenu;
    },
    itemClicked: function(item) {
      this.activeOption = item.abbr;
      this.activeItem = item;
      this.toggleShow();
      this.url = '';
    },
    onSearch: function() {
      let store = this.activeItem.store;
      let cb_id = this.activeItem.cb_id;
      let currency = this.activeItem.currency;
      this.url = `https://store.acronis.com/${store}/purl-consumer-standard-US?cart=${cb_id}&currencies=${currency}&language=en`
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

a {
  color: #42b983;
}

.search {
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid transparent;
    padding: .75rem 2rem;
    font-size: 1rem;
    border-radius: .25rem;
    transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
    color: #fff;
    background-color: #27AE60;
    border-color: #27AE60;
    margin-top: 10px;
}

.search:hover {
  color: #fff;
  background-color: #229954;
  border-color: #229954;
  cursor: pointer;
}

.anchor {
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid transparent;
    padding: .75rem 2rem;
    font-size: 1rem;
    border-radius: .25rem;
    transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
    color: #fff;
    background-color: #27AE60;
    border-color: #27AE60;
}

.anchor::after {
    display: inline-block;
    width: 0;
    height: 0;
    margin-left: .5em;
    vertical-align: .255em;
    content: "";
    border-top: .3em solid;
    border-right: .28em solid transparent;
    border-bottom: 0;
    border-left: .28em solid transparent;
}

.anchor:hover {
  color: #fff;
  background-color: #229954;
  border-color: #229954;
  cursor: pointer;
}

.menu {
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid rgba(0,0,0,.15);
  border-radius: .25rem;
  color: #212529;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  font-size: 1rem;
  list-style: none;
  margin: .125rem 0 0;
  padding: .5rem 0;
  position: absolute;
  text-align: left;
}

.menu-item {
  color: #212529;
  padding: .25rem 1.5rem;
  transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
}

.menu-item:hover {
  background-color: #F4F6F6;
  cursor: pointer;
}


</style>
