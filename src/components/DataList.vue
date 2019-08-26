<template>
  <div id="container" align="left">
    <h2>Access Level</h2>
    <div class="listb">
        <el-input v-model="searchFilterText"  @keyup.native="filterText" placeholder="Spotlight Search" class="txfilt"></el-input>
        <el-table :data=tableData stripe @row-click="handleRowSelect">
          
          <el-table-column prop="name" label="Name">
            <span slot-scope="scope">{{ scope.row.name }}</span>
          </el-table-column>
          <el-table-column prop="readerType" label="Reader Type"></el-table-column>
          <el-table-column prop="reader" label="Reader(s)"></el-table-column>
          
        </el-table>
    </div>
    <access-detail :readers="readers" :selectRow="selectRow" v-on:saveTable="saveTable"></access-detail>
  </div>
</template>

<script>
  import AccessDetail from "./AccessDetail.vue"
  export default {    
    data() {
        return {
            tableData: getTable(),
            selectRow: {
                id: 0,
                name: '',
                readerType: '',
                reader: '',
                desc:""
            },
            readers: getReaders(),
            searchFilterText: "",
            tableClone: []            
        }

    },
    components: {
      AccessDetail
    },
    mounted() {
      this.tableClone = this.tableData;
    },
    methods: {
       handleRowSelect(row, event, column) {
           this.selectRow=row
        },
        saveTable(selectRow) {
          this.selectRow=selectRow
          if(this.selectRow.id>0) 
          for(var i=0;i<this.tableData.length;i++) {
            if(this.tableData[i].id===this.selectRow.id) {
             
              this.tableData[i].name=this.selectRow.name
              this.tableData[i].desc=this.selectRow.desc
              this.tableData[i].reader=this.selectRow.reader
              break;
            }
          }
        },
        filterText(event) {
          
            let val=this.searchFilterText
            this.tableData = this.tableClone;
            
            this.tableData = this.tableData.filter((row) => {
              return row.name.toLowerCase().includes(val.toLowerCase());
            });
            if(this.tableData.length<=0) {
                this.tableData = this.tableClone;
                this.tableData = this.tableData.filter((row) => {
                return row.reader.toLowerCase().includes(val.toLowerCase());
              });
            }
            if(this.tableData.length<=0) {
                this.tableData = this.tableClone;
                this.tableData = this.tableData.filter((row) => {
                return row.readerType.toLowerCase().includes(val.toLowerCase());
              });
            }
        }


      }

  }

  //creat table data as name,reader type and reader based on 3 set of raw data.
  function getTable() {
    var datab=[]
    var t_data=getData();
    var reader_data=getReaders()
    var readtype_data=getReaderTypes()
    var realTable={}

    for(var i=0;i<t_data.length;i++)  {
      realTable={
                id: 0,
                name: '',
                readerType: '',
                reader: '',
                desc:""
            }

      realTable.id=t_data[i].id
      realTable.name=t_data[i].name
      realTable.desc=t_data[i].Description
     
      var rtypeId=''
      realTable.reader=reader_data.find(x => x.id === t_data[i].readerId).name
      rtypeId=reader_data.find(x => x.id === t_data[i].readerId).typeId
      realTable.readerType=readtype_data.find(y => y.id === rtypeId).name

      datab.push(realTable)
    }
    
    return datab
  }

  function getReaders() {
    return [{
      "id": 10,
      "typeId": 1,
      "name": "Reader F1"
    },
    {
      "id": 11,
      "typeId": 1,
      "name": "Reader F2"
    },
    {
      "id": 12,
      "typeId": 1,
      "name": "Reader F3"
    },
    {
      "id": 13,
      "typeId": 1,
      "name": "Reader F4"
    },
    {
      "id": 14,
      "typeId": 1,
      "name": "Reader F5"
    },
    {
      "id": 15,
      "typeId": 2,
      "name": "Reader E"
    },
    {
      "id": 16,
      "typeId": 3,
      "name": "Reader B"
    },
    {
      "id": 17,
      "typeId": 4,
      "name": "Reader S"
    }
  ]

  }

  function getReaderTypes() {
    return [{
      "id": 1,
      "name": "Front Door"
    },
    {
      "id": 2,
      "name": "Elevator Door"
    },
    {
      "id": 3,
      "name": "Back Door"
    },
    {
      "id": 4,
      "name": "Supply Door"
    }
  ]

  }

  function getData() {
  return [{
      "id": 1,
      "name": "Morning 9:00 - 10:00",
      "readerId": 10,
      "Description": "Morning Front door Access"
    },
    {
      "id": 2,
      "name": "Morning 10:00 - 11:00",
      "readerId": 11,
      "Description": ""
    },
    {
      "id": 3,
      "name": "Morning 11:00 - 12:00",
      "readerId": 12,
      "Description": ""
    },
    {
      "id": 4,
      "name": "Evening 3:00 - 4:00",
      "readerId": 13,
      "Description": ""
    },
    {
      "id": 5,
      "name": "Evening5:00 - 6:00",
      "readerId": 14,
      "Description": ""
    },
    {
      "id": 6,
      "name": "All Day Elevator",
      "readerId": 15,
      "Description": ""
    },
    {
      "id": 7,
      "name": "All Day Back Door",
      "readerId": 16,
      "Description": ""
    },
    {
      "id": 8,
      "name": "All Day Supply Door",
      "readerId": 17,
      "Description": ""
    }
  ]
  }

</script>

<style>
  #container {
    width:99%;
    border: 2px solid #A9A9A9; 
    padding:5px;
    display: inline-block;
    padding:5px;

  }
  .listb{
    width:50%;
    border: 1px solid #DCDCDC;
    float:left;
  }
  .txfilt {
    width:99%;
    height: 40px;
  }
</style>