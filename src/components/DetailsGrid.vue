<template>
  <div class="detailsGrid">
    <div class="main">
       <table>
        <thead>
          <tr>
            <th v-for="key in columns"  v-bind:key="key.name" :class="{ show:key.show }">
              {{ key.header }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(entry, index) in dataActions" v-bind:key="index">
            <td v-for="key in columns" v-bind:key="key.name" :class="{ show:key.show }">
                <div v-if="key.edit && key.type == 'number'">
                    <input :value="entry[key.name]" :data-row="index" :data-column="key.name" :data-type="key.type" :data-past="entry[key.name]" 
                      @change="onChange" :maxlength="parseInt(key.maxlength)" max= 99 min= 0 :type="key.type" class="num"/>
                </div>
                <div v-if="key.edit && key.type != 'number'">
                    <input :value="entry[key.name]" :data-row="index" :data-column="key.name" :data-type="key.type" @change="onChange" :maxlength="parseInt(key.maxlength)" :type="key.type"/>
                </div>
                <div v-if="!key.edit">
                  {{entry[key.name]}}
                </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    
  </div>
</template>

<script>
export default {
  name: 'detailsGrid',
  props: {
    actions: Array,
    columns: Array,
  },
  computed: {
    dataActions: function () {
      return this.actions
    }
  },
  methods: {
    onChange: function(ev) {
        let col = ev.target.dataset.column
        let row = ev.target.dataset.row
        let type = ev.target.dataset.type
        if(type == "number"){
            let past = ev.target.dataset.past
            let newVal = parseInt(ev.target.value)
            if(newVal > 99 || newVal < 0){
                this.actions[parseInt(row)][col] = past
            }else{
                this.actions[parseInt(row)][col] = parseInt(ev.target.value)
            }
        }else{
            this.actions[parseInt(row)][col] = ev.target.value
        }
    },
  }
}
</script>
<style scoped>
main {
    font-family: Helvetica Neue, Arial, sans-serif;
    font-size: 10px;
    color: #444;
    align-self: right;
  }
  
  table {
    background-color: #FFF;
    align-self: right;
    direction: rtl;
  }
  
  th {
    background-color: #EBFF89;
    color: #000;
    cursor: pointer;
  }
    tr {
        background-color: #FFF;
    }
    tr:nth-child(even){
        background-color: #D0D0D0;
    }
  
  th, td {
    max-width: 150px;
    min-width: 20px;
    padding: 10px 20px;
  }
  
  th.active {
    color: #aaa;
  }
  input{
    width: 150px;
  }
  input.num{
    width: 30px;
  }
 
  @media only screen and (max-width: 300px) {
    td.show {
      display:none;
      width: 0px;
      height: 0px;
      opacity:0;
      visibility: collapse;	
      padding: 0px;	
    } 
  th.show {
    display:none;
    width: 0px;
    height: 0px;
    opacity:0;
    visibility: collapse;
    max-width: 0px;
    padding: 0px;	
  }
}
@media screen 
  and (device-width: 360px) 
  and (device-height: 640px) 
  and (-webkit-device-pixel-ratio: 4) 
  and (orientation: portrait) {

  table, thead, tbody, th, td, tr { 
    display: block; 
  }

  tr { border: 1px solid #ccc; }
	
	td { 
		/* Behave  like a "row" */
		border: none;
		border-bottom: 1px solid #eee; 
		position: relative;
		padding-left: 50%; 
	}
  td:before { 
		/* Now like a table header */
		position: absolute;
		/* Top/left values mimic padding */
		top: 6px;
		left: 6px;
		width: 45%; 
		padding-right: 10px; 
		white-space: nowrap;
	}
  thead tr { 
		position: absolute;
		top: -9999px;
		left: -9999px;
	}

}
</style>
