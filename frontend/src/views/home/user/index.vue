<template>
  <div class="app-container">
    <!-- $t is vue-i18n global function to translate lang -->
    <img src="./home.png" width="100%">
  </div>
</template>
<script>
import { getDish,order,getAllDishOrderedByEmpl, paid } from "@/api/article";

import { getInfor } from "@/api/remote-search";

export default {
  name: "SelectExcel",
  data() {
    return {
      numberAccecptPay: 0,
      isAdminOrderd: false,
      pay: -1,
      listOrderedTemporOrdered: null,
      totalPrice: 0,
      listOrderedTemp: null,
      listOrdered: null,
      list: null,
      listLoading: true,
      multipleSelection: [],
      downloadLoading: false,
      filename: "",
      originNumber: 0,
      submit: false,
      dialogFormVisible: false,
      infor:{
        id: 1,
        content:''
      }

    };
  },
  created() {
    this.fetchData();
    this.getDishOrdered();
    this.getInfor()
  }, 
  methods: {
    fetchData() {
      this.listLoading = true;
      getDish().then((response) => {     
        this.list = response.data[0];
      });
      setTimeout(() => {
          this.listLoading = false
        }, 1.5 * 1000)
    },
    getDishOrdered(){
      this.listLoading = true;
      getAllDishOrderedByEmpl().then((response) => {
        this.listOrdered = response.data;
        this.isOrdered()
        this.totalPrice =  this.computeToTalPrice(this.listOrderedTemporOrdered)
      });
      setTimeout(() => {
          this.listLoading = false
        }, 1.5 * 1000)
    },
    isOrdered(){

      if (this.listOrdered.length > 0){
        this.listOrderedTemporOrdered = this.listOrdered;
        this.submit = true
        if(this.listOrdered[0][2] != 0){
          this.isAdminOrderd = true;
        }
        if(this.listOrdered[0][8] == 0){
          this.pay = 0
        }
        this.checkPay(this.listOrdered)
      }else{
        this.listOrderedTemporOrdered = this.listOrderedTemp;
      }
      
    },
    getInfor(){
      getInfor().then((response) => {
        // this.infor.id = response.data[0].id;
        // this.infor.content = response.data[0].content;
      });

    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
      this.listOrderedTemp = this.multipleSelection;
      this.isOrdered()
      this.totalPrice = this.computeToTalPrice(this.listOrderedTemporOrdered)
    },
    handlePay() {
      this.pay = 0;
      var obj = {
        id: null
      }
      paid(obj).then((response) => {
        this.$message({
          message: "Đang Chờ Xác Nhận",
          type: "success",
        });
      });
    },
    handleSubmit() {
      if(this.multipleSelection.length){
        this.submit = true;
        const data = this.formatJson(this.listOrderedTemporOrdered)
        console.log(data)

        order(data).then(() => {
          this.getDishOrdered()
        });
        this.$message({
          message: "Đã Đặt Món Ăn",
          type: "success",
        });
      }else{
        this.$message({
          message: "Chưa Chọn Món Ăn",
          type: "warning",
        });
      }
      
    },
    formatJson(jsonData){
      return jsonData.map((item) => {
        return {
          dish_id: item[0],
          name: item[1],
          price: item[2],
          image_link: item[3],
          id_store: item[4],
          store_name: item[5],
          link: item[6],
          quantity: item[7],
        }
      })
    },
    handleEdit() {
      this.submit = false;
    },
    checkPay(jsonData){
      return jsonData.map((item) => {
        if(item[8]==1){
          this.numberAccecptPay +=1
          return this.pay = 1;
        }
      })
    },
    computeToTalPrice(listOrdered) {
      var totalPrice = 0;
      if(listOrdered == null) return;
      listOrdered.forEach((item) => {
        if(typeof item[2] == "number"){
          if(item[2] !=0){
            totalPrice += item[2] * item[7];
          }else{
            totalPrice += item[9] * item[7];
          }
        }else{
        totalPrice += Number(item[2].replace(",","")) * Number(item[7]);
        }
      });
      return totalPrice;
    }
  }
};
</script>