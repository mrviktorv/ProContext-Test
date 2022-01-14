<template>

<div id="wrapper">

  <div id="left-window" >
    <dl>
      <div v-for="list in lists" :key="list.id" class="list-div">
        <dt>
          <i class="arrow" id="toggleHide" ref="toggleHide" v-bind:style="{ transform: list.rotation }" @click="hideItems(list)"></i>
          <input type="checkbox" v-indeterminate="Indeterm(list)" v-model="list.selectAll" @click="selectList(list)"> 
          {{ list.listName }}
        </dt>
        <dd v-for="item in list.items" :key="item.id" v-bind:style="{ display: list.visibility }">
          <input type="checkbox" v-model="list.selected" :value="item.id" number >
          {{ selectItem(list) }}
          {{ item.name }}
          <input type="number" class="item-input-number" min="0" oninput="validity.valid||(value='');" v-model="item.number"> 
          <input type="color" v-model="item.color">
        </dd>
      </div>
    </dl>
  </div>

  <div id="right-window" >
    <div class="inner" v-for="list in lists" :key="list.id" >
      {{ list.listName }}
      <div v-for="item in list.items" :key="item.id">
        <div v-if="list.selected.indexOf(item.id) > -1">
          <span v-for="n in parseInt(item.number)" :key="n.id">
            <span class="square" v-bind:style="{ color: item.color }" @click="deleteItem(item)">
              &#9632;
            </span>
          </span>
        </div>
      </div>
    </div>
  </div>
 
</div>

</template>

<script>
export default {
  name: 'App',
  components: {
  },
  data: function () {
    return {
      lists: [
        {
        listName: 'List 1',
        selectAll: false,
        selected: [],
        rotation: 'rotate(45deg)',
        visibility: 'block',
        items: [ 
            { "id": "1", "name": "Item 1", "number": "2", "color" : "#9E26EE" },
            { "id": "2", "name": "Item 2", "number": "6",  "color" : "#FCB019" }, 
            { "id": "3", "name": "Item 3", "number": "13",  "color" : "#3A7994"}, 
            { "id": "4", "name": "Item 4", "number": "5",  "color" : "#ff0000" }
        ],
        },
        {
        listName: 'List 2',
        selectAll: false,
        selected: [],
        rotation: 'rotate(45deg)',
        visibility: 'block',
        items: [ 
            { "id": "5", "name": "Item 1", "number": "4", "color" : "#9E26EE" },
            { "id": "6", "name": "Item 2", "number": "15",  "color" : "#FCB019" }, 
            { "id": "7", "name": "Item 3", "number": "32",  "color" : "#3A7994"}, 
            { "id": "8", "name": "Item 4", "number": "5",  "color" : "#ff0000" },
            { "id": "9", "name": "Item 5", "number": "25",  "color" : "#00FF00" }
        ],
        },
        {
        listName: 'List 3',
        selectAll: false,
        selected: [],
        rotation: 'rotate(45deg)',
        visibility: 'block',
        items: [ 
            { "id": "10", "name": "Item 1", "number": "3", "color" : "#9E26EE" },
            { "id": "11", "name": "Item 2", "number": "7",  "color" : "#008080" }, 
            { "id": "12", "name": "Item 3", "number": "1",  "color" : "#3A7994"}, 
            { "id": "13", "name": "Item 4", "number": "5",  "color" : "#300000" },
            { "id": "14", "name": "Item 5", "number": "8",  "color" : "#FF9090" },
            { "id": "15", "name": "Item 6", "number": "40",  "color" : "#ff0000" }
        ],
        },
      ],
    }
  },
  directives: {
    indeterminate: function(el, binding) {
      el.indeterminate = Boolean(binding.value)
    }
  },
  methods: {
    Indeterm: function(list) {    
      if (list.selected.length < list.items.length && list.selected.length > 0) 
      {
        return true;
      }
      else
      {
        return false;
      }
    },
    selectItem: function(list) {
      if (list.selected.length == list.items.length) {
        list.selectAll = true
      } else {
        list.selectAll = false}
    },
    selectList: function(list) {
      if (list.selectAll == true) {
      list.selected = []
      } else {
        let selected = [];
          list.items.forEach(function (item) {
              selected.push(item.id);
          });
      list.selected = selected;
      }
    },
    deleteItem: function(item) {
      let number = parseInt(item.number);
      number--;
      item.number = (number).toString()
    },
    hideItems: function(list) {
      if (list.rotation == 'rotate(45deg)') {
      list.rotation = 'rotate(-45deg)';
      list.visibility = 'none'
      } else {
      list.rotation = 'rotate(45deg)';
      list.visibility = 'block'
      }
    } 
  },
}
</script>

<style>
#wrapper {
  border: 1px solid black;
  overflow: hidden; 
  display: flex;
}

#left-window {
  border-style: solid; 
  border-width: 1px; 
  border-color: black;
  float: left;
  width: 40%;
  padding: 10px;
  margin: 20px;
  margin-right: 5%;
}

#right-window {
  border-style: solid; 
  border-width: 1px; 
  border-color: black;
  float: right;
  width: 40%;
  padding: 10px;
  margin: 20px;
  margin-left: 10%;
}

.inner {
  border-style: solid; 
  border-width: 1px; 
  border-color: black;
  padding: 10px;
  margin: 10px;
}

.square:hover {
  cursor: pointer;
}

.square {
  font-size: 20px;
}

.list-div {
  margin: 10px;
}

.arrow {
  border: solid black;
  border-width: 0 2px 2px 0;
  display: inline-block;
  padding: 4px;
  margin-right: 4px;
  cursor: pointer;
}

.item-input-number {
  width: 4em;
  margin-left: 13em;
  margin-right: 1em;
}
</style>
