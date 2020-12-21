<template>
  <div class="summaryGrid">
    <div class="main">
       <table>
        <tbody>
          <tr v-for="entry in dataActions" v-bind:key="entry.time">
            <td v-for="key in entry" v-bind:key="key.name" >
              <div> {{key}}</div> 
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    
  </div>
</template>

<script>

export default {
  name: 'summaryGrid',
  props: {
    actions: Array,
  },
  data: function () {
    return {
    }
  },
  computed: {
    dataActions: function () {
      return this.actions
    }
  },
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
    background-color: #fff;
    align-self: right;
    direction: rtl;
  }
  
  tr {
      background-color: #fff;
  }
  
  th, td {
    min-width: 50px;
    padding: 10px 20px;
    color: #000;
  }
  td:nth-child(1){
    background-color: #EBFF89;
  }
  td:nth-child(2){
    background-color: #D0D0D0;
  }

</style>
