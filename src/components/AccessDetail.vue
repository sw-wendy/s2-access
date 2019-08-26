<template>
	<div id="detailb">
      
      <h2>{{title}}</h2>
      
      Name:<el-input type="text" v-model="selectRow.name" ></el-input>
      Desc:<el-input type="textarea" :rows="3" v-model="selectRow.desc"></el-input>
      
      Reader(s):<el-select v-model="selectRow.reader" placeholder="All Reader Groups">
            <el-option v-for="item in readers" :key="item.id" :value="item.name" :label="item.name">
            </el-option>
          </el-select>
      <div class="eleitem">
      <el-button @click="saveChange">Save</el-button>
      <el-button @click="cancelChange">Cancel</el-button>
      </div>

    </div>

</template>

<script>
  export default {
  	props: ["readers","selectRow"],    
    data() {
        return {            
            title:"Access Name"
        }
    },
    methods: {
    	saveChange() {
    		let self=this
    		this.$emit("saveTable", self.selectRow)
    	},
    	cancelChange() {
    		    
    	}
      }

  }

  
</script>

<style>
  #detailb {
    width:47%;
    float:right;
    height: 480px;
  }

  .eleitem {
    padding:10px;
  }
  .txfilt {
    width:99%;
    height: 40px;
  }
</style>
