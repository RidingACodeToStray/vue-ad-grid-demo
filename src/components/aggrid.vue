<template>
  <div>
      <div>总数:{{rowData.length}}条</div>
      <button @click="showCol('DEPT_CD')">显示DEPT_CD</button>
      <button @click="hideCol('DEPT_CD')">隐藏DEPT_CD</button>
      <button @click="seeData">查看数据</button>
    <ag-grid-vue style="width: 100%; height: 500px;"
                 class="ag-theme-balham"
                 :enableRangeSelection="true"
                 :gridOptions="gridOptions"
                 :columnDefs="columnDefs"
                 :rowSelection="rowSelection"
                 :rowData="rowData">
    </ag-grid-vue>
   <!-- :suppressRowTransform="true"  //允许行合并 -->
   <!-- :enableRangeSelection="true"  //批量选中,需要安装和引入企业版ag-grid-enterprise -->
  </div>
</template>

<script>
    import {AgGridVue} from "ag-grid-vue";
    import "ag-grid-enterprise";
    export default {
        name: 'App',
        data() {
          //第二列下拉的的赋值绑定
          let colo2Map = {
              tyt: "Toyota",
              frd: "Ford",
              prs: "Porsche",
              nss: "Nissan"
          }
          function extractValues(mappings){
            return Object.keys(mappings);
          }
          function lookupValue(mappings, key) {
            return mappings[key];
          }
          function lookupKey(mappings, name) {
            for (var key in mappings) {
              if (mappings.hasOwnProperty(key)) {
                if (name === mappings[key]) {
                  return key;
                }
              }
            }
          }
          function numberValueSetter(params) {
            if (isNaN(parseFloat(params.newValue)) || !isFinite(params.newValue)) {
              return false;
            }
            params.data.price = params.newValue;
            return true;
          }
          //数据格式
          function currencyFormatter(params) {
            var value = Math.floor(params.value);
            if (isNaN(value)) return "";
            return "\xA3" + value.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1,");
          }
          function numberValueSetter(params) {
            if (isNaN(parseFloat(params.newValue)) || !isFinite(params.newValue)) {
              return false;
            }
            params.data.EMP_DUTY = params.newValue;
            return true;
          }


            return {
              rowSelection:"multiple",  //设置单行single或多行选中multiple
              gridOptions:{
                defaultColDef: {
                  resizable: true,  //列宽可拖拽
                  editable: true,  //可编辑
                },
                columnApi:null,  //设置列隐藏和显示的参数
              },
              columnDefs: [
                    {
                      headerName: 'CO_CD', 
                      field: 'CO_CD',
                      headerCheckboxSelection: true,  //在表头添加chcekbox
                      headerCheckboxSelectionFilteredOnly: true, //复选框将在选中时仅选择已筛选的行，并在取消选中时仅取消选择已筛选的行
                      checkboxSelection: true,  //给当前行添加chckbox
                    },
                    {
                      headerName: 'CO_x', 
                      field: 'CO_x',
                      cellEditor: "select",
                      cellEditorParams: { values: extractValues(colo2Map) },
                      valueFormatter: params => {
                        return lookupValue(colo2Map, params.value);
                      },
                      valueParser: function(params) {
                        return lookupKey(colo2Map, params.newValue);
                      }
                    },
                    {headerName: 'Group A',
                        children: [
                            {headerName: 'DEPT_CD', field: 'DEPT_CD'},
                            {headerName: 'DIV_CD', field: 'DIV_CD'},
                        ]
                    },
                    {
                      headerName: 'EMP_DUTY', 
                      field: 'EMP_DUTY',
                      colId: "retailPrice",
                      valueGetter: function(params) {
                        return params.data.EMP_DUTY;
                      },
                      valueFormatter: currencyFormatter,
                      valueSetter: numberValueSetter
                    },
                ],
                rowData: [
                  { CO_CD:11,CO_x:'frd',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'prs',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'tyt',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'nss',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'tyt',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'frd',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'frd',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'nss',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'frd',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'tyt',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'tyt',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'prs',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'tyt',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'nss',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'nss',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'frd',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'tyt',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'nss',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'tyt',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'tyt',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'frd',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'prs',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'tyt',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                  { CO_CD:11,CO_x:'prs',DEPT_CD:2,DIV_CD:3,EMP_DUTY:5,EMP_JOB:6,EMP_NATIVE:7,EMP_NM:8,},
                ]
            }
        },
        components: {
            AgGridVue
        },
        methods:{
          showCol(field){
            this.gridColumnApi.setColumnVisible(field, true);
          },
          hideCol(field){
            this.gridColumnApi.setColumnVisible(field, false);
          },
          seeData(){
            console.log(111,this.rowData)
          },

          extractValues(mappings) {
            return Object.keys(mappings);
          },
          lookupValue(mappings, key) {
            return mappings[key];
          },
          lookupKey(mappings, name) {
            for (var key in mappings) {
              if (mappings.hasOwnProperty(key)) {
                if (name === mappings[key]) {
                  return key;
                }
              }
            }
          },
        },
    }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  @import "../../node_modules/ag-grid-community/dist/styles/ag-grid.css";
  @import "../../node_modules/ag-grid-community/dist/styles/ag-theme-balham.css";
  .ag-header-group-text,
  .ag-header-cell-text{
    width: 100%;
  }
  .cell-span{
    background: #e3e3e3;
  }
</style>
