<template>
<div class="grid-container">
    <div class="grid-item">
        <detailsGrid
                :actions="ordersGridData"
                :columns="ordersGridColumns">
            </detailsGrid>
    </div>
    <div class="grid-item">
        <summaryGrid
                :actions="ordersSums()">
            </summaryGrid>
    </div>
</div>
</template>

<script>
import DetailsGrid from './DetailsGrid.vue'
import SummaryGrid from './SummaryGrid.vue'
import axios from 'axios'

const OrdersURL = 'https://localhost:44320/api/orders'

export default {
  name: 'Orders',
  data() {
    return{
        ordersGridColumns:  [ {header:'מזהה הזמנה', name:'OrderID', type:"text"},
                                {header:'שם לקוח', name:'CompanyName', type:"text", edit:"true", maxlength: 30},
                                {header:'כתובת משלוח', name:'Address', type:"text", show:"false"},
                                {header:'עיר משלוח', name:'City', type:"text", show:"false"},
                                {header:'אזור משלוח', name:'Region', type:"text", show:"false"},
                                {header:'עלות משלוח', name:'ShippingCost', type:"number", edit:"true", maxlength: 2}],
        ordersGridData: [],
        bold: true
      }
  },
  components: {
      DetailsGrid,
      SummaryGrid
  },
  mounted: function(){
    var config = {
        headers: {'Access-Control-Allow-Origin': '*',
                    'Content-Type': 'application/json;charset=utf-8'}
    };
    axios.get(OrdersURL, config).then((response) => {
        this.ordersGridData = response.data
    }).catch(function (error) {
        console.error(error);
    })
  },
  methods: {
    ordersSums: function(){
        let sumOfShippingCost = 0;
        this.ordersGridData.forEach(element => {
              sumOfShippingCost += parseInt(element.ShippingCost)
          });
        let ammountOfShipping = this.ordersGridData.length;
        let averageShippingCost = ammountOfShipping ?  sumOfShippingCost / ammountOfShipping : 0;

        return [ {name:'סה"כ כמות הזמנות', num:ammountOfShipping},
                    {name:'סה"כ עלות משלוח', num:sumOfShippingCost},
                    {name:'ממוצע עלות משלוח', num:averageShippingCost}]
    }
  }
}
</script>
<style scoped>
    .grid-container {
        display: grid;
        grid-template-columns: auto auto;
    }
    @media only screen and (max-width: 600px) {
        .grid-container {
            display: grid;
            grid-template-columns: auto;
        }
    }
</style>