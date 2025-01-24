<template>
   <div v-if="dataLoaded" class="realtime_container">
      <div class="realtime_container_btns">

         <div class="realtime_container_btns_checkbox">
            <div v-for="item in [0, 500, 1000, 3000]" class="realtime_btns_checkbox">
               <label :id="item + 'DelayCheckbox'">
                  <input @click="intervalData(item)" v-model="delay" :value="item" type="radio" class="">
                  <div v-if="item !== 0" class="">{{ item / 1000 + 's' }}</div>
                  <div v-else class="">Off</div>
               </label>
            </div>
         </div>
         <div class="realtime_container_btns_views">
            <button v-for="btn in statsButtons" @click="changeView(btn.name)" :id="btn.name"
               class="realtime_btns_views">{{ btn.name }}</button>
         </div>
      </div>
      <div class="realtime_container_views">
         <div class="realtime_container_views_tdma_frameplan_container" v-if="tdmaOverviewActive">
            <div class="realtime_container_views_tdma_frameplan_chart">
               <div class="tdma_realtime_container_views_tdma_frameplan_column_char_req">
                  <div class="tdma_realtime_container_views_tdma_frameplan_column_char_req_container_label_axis"
                     :style="{ 'grid-template-rows': gridTemplateRowsOverview }">
                     <div class="requested_label">
                        Requested
                     </div>
                     <div class="QoS_label">
                        QoS requested
                     </div>
                     <div class="allocated_label">
                        Allocated
                     </div>
                     <div class="framelayout_label">
                        Frame Layout
                     </div>
                  </div>
                  <!-- base -->

                  <div class="tdma_realtime_container_views_tdma_frameplan_column_char_req_container"
                     id="containerGridTDMA"
                     :style="{ 'grid-template-rows': tdmaGenerator_row_column['grid-template-rows'], 'grid-template-columns': tdmaGenerator_row_column['grid-template-columns'] }">
                     <div class="tdma_labels_top">
                        <div v-for="label in labelsTop" class="container_label">
                           <div :style="{ 'background': label.color }" class="circle_label"></div>
                           <div class="text_label_tdma">{{ label.name }}</div>
                        </div>
                     </div>
                     <div v-for="(req, idxReq) in tdmaesqueletorGeneratorChartRectangleAxisHorizontalLine()"
                        id="horizontalAxis" :style="{ 'grid-area': req['grid-area'], 'border-top': req['border'] }">
                     </div>
                     <div v-for="(req, idxReq) in tdmaesqueletorGeneratorChartRectangleAxistinyLineunderNumber()"
                        :style="{ 'grid-area': req['grid-area'], 'border-left': req['border'] }" id="verticalAxisLine">
                     </div>
                     <div v-for="(req, idxReq) in numberAxis"
                        :style="{ 'grid-area': req['grid-area'], 'justify-content': req['position'] }" id="axis1"
                        class="realtime_rql">
                        {{ req['value-axis'] }}
                     </div>
                     <div class="tdma_labels_bottom">
                        <div v-for="label in labelsBottom" class="container_label">
                           <div :style="{ 'background': label.color }" class="circle_label"></div>
                           <div class="text_label_tdma">{{ label.name }}</div>
                        </div>
                     </div>
                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleRequestedRTValues"
                        :id="`${idxReq}-RTValue`" class="" :style="{
                           'grid-area': req['grid-area'],
                           'background-color': req['color'],
                           'border-top': req['border'],
                           'border-bottom': req['border'],
                           'border-left': req['border-left'],
                           'border-right': req['border-right'],
                        }">
                     </div>
                     <div v-for="(req, idxReq) in RTValues" class="values_tdmaOverview" id="RTValues" :style="{
                        'grid-area': req['grid-area'],
                        'color': req['color']
                     }">
                        {{ req.value }}
                     </div>

                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleRequestedValues"
                        id="RequestedColor class=" :style="{
                           'grid-area': req['grid-area'], 'background-color':
                              req['color'], 'border-top': req['border'], 'border-bottom': req['border'], 'border-left':
                              req['border-left'], 'border-right': req['border-right'],
                        }">
                     </div>
                     <div v-for="(req, idxReq) in RequestedValues" class="values_tdmaOverview" id="RequestedValues"
                        :style="{
                           'grid-area': req['grid-area'],
                           'color': req['color']
                        }">
                        {{ req.value }}
                     </div>
                     <!--                      <div :style="{
                        'grid-area': `3 / ${controllerStruct.total_slots+1} / 3 / ${(controllerStruct.total_slots+1)}`,
                        'background-color':'pink'
                     }">
                       
                     </div>
                      <div :style="{'grid-area': `3 / ${controllerStruct.total_slots+2} / 3 / ${controllerStruct.total_slots+2}`,
                        'background-color':'cyan'
                     }">
                     </div>
                    <div :style="{'grid-area': `3 / ${controllerStruct.total_slots+3}/ 3 / ${controllerStruct.total_slots+3}`,
                      'background-color':'red'
                    }">
                     </div> -->

                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleRequestedCIRValues" class=""
                        :style="{
                           'grid-area': req['grid-area'],
                           'background-color': req['color'],
                           'border-top': req['border'],
                           'border-bottom': req['border'],
                           'border-left': req['border-left'],
                           'border-right': req['border-right'],
                        }">
                     </div>
                     <div v-for="(req, idxReq) in CIRValues" class="values_tdmaOverview" id="CIRValues" :style="{
                        'grid-area': req['grid-area'],
                        'color': req['color']
                     }">
                        {{ req.value }}
                     </div>

                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleRequestedALLOCATEDValues" class=""
                        :style="{ 'grid-area': req['grid-area'], 'background-color': req['color'] }">
                     </div>
                     <div v-for="(req, idxReq) in ALLOCATEDValues" class="values_tdmaOverview" id="ALLOCATEDValues"
                        :style="{ 'grid-area': req['grid-area'] }">
                        {{ req.value }}
                     </div>

                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleFREEValues" class=""
                        :style="{ 'grid-area': req['grid-area'], 'background-color': req['color'] }">
                     </div>
                     <div v-for="(req, idxReq) in FREEValues" id="FREEValue" class="values_tdmaOverview"
                        :style="{ 'grid-area': req['grid-area'] }">
                        {{ req.value }}
                     </div>

                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleIDLEValues" class=""
                        :style="{ 'grid-area': req['grid-area'], 'background-color': req['color'] }">
                     </div>
                     <div v-for="(req, idxReq) in IDLEValues" class="values_tdmaOverview" id="IDLEValues"
                        :style="{ 'grid-area': req['grid-area'] }">
                        {{ req.value }}
                     </div>
                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleDATAValues" class=""
                        :style="{ 'grid-area': req['grid-area'], 'background-color': req['color'] }">
                     </div>
                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleACQUISITIONValues" class=""
                        :style="{ 'grid-area': req['grid-area'], 'background-color': req['color'] }">
                     </div>
                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleZEROValues" class=""
                        :style="{ 'grid-area': req['grid-area'], 'background-color': req['color'] }">
                     </div>



                     <div v-for="(req, idxReq) in DATAValues" class="values_tdmaOverview" id="DATAValues"
                        :style="{ 'grid-area': req['grid-area'] }">
                        {{ req.value }}
                     </div>
                     <div v-for="(req, idxReq) in ACQUISITIONValues" class="values_tdmaOverview" id="ACQUISITIONValues"
                        :style="{ 'grid-area': req['grid-area'] }">
                        {{ req.value }}
                     </div>
                     <div v-for="(req, idxReq) in ZeroValues" class="values_tdmaOverview" id="ZeroValues"
                        :style="{ 'grid-area': req['grid-area'] }">
                        {{ req.value }}
                     </div>
                  </div>
                  <div class="tdma_realtime_container_views_tdma_frameplan_column_char_req_container_overflow"
                     id="containerGridTDMAOVERFLOW"
                     :style="{ 'grid-template-rows': tdmaGenerator_row_column_overflow['grid-template-rows'], 'grid-template-columns': tdmaGenerator_row_column_overflow['grid-template-columns'] }">
                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleOverflowValuesRequested"
                        id="overflowcolorRequested" :style="{
                           'grid-area': req['grid-area'],
                           'background-color': req['color'],
                           'border-top': req['border'],
                           'border-bottom': req['border'],
                           'border-left': req['border-left'],
                           'border-right': req['border-right']
                        }">
                     </div>
                     <div v-for="(req, idxReq) in OverflowValuesRequested" class="values_tdmaOverview"
                        id="OverflowValuesRequested" :style="{
                           'grid-area': req['grid-area'],
                           'color': req['color']
                        }">
                        {{ req.value }}
                     </div>
                     <div v-for="(req, idxReq) in tdmasqueletorGeneratorChartRectangleOverflowValuesFrameLayout"
                        id="overflowcolorFrameLayout"
                        :style="{ 'grid-area': req['grid-area'], 'background-color': req['color'] }">
                     </div>
                     <div v-for="(req, idxReq) in OverflowValuesFramelayout" class="values_tdmaOverview"
                        id="OverflowValuesFramelayout" :style="{ 'grid-area': req['grid-area'] }">
                        {{ req.value }}
                     </div>
                  </div>
                  <div class="tdma_realtime_container_views_tdma_frameplan_column_char_req_container_overflow_arrow"
                     id="containerGridTDMAOVERFLOWArrow"
                     :style="{ 'grid-template-rows': tdmaGenerator_row_column_overflow['grid-template-rows'], 'grid-template-columns': '1fr' }">
                     <div v-if="overflow_rt_req()" class="tdma_realtime_container_views_tdma_frameplan_column_arrow">
                        &rarr;
                     </div>

                  </div>
                  <div v-if="overflowtype1"
                     class="tdma_realtime_container_views_tdma_frameplan_column_char_req_container_overflow_requested_text"
                     :style="{ 'grid-template-rows': gridTemplateRowsOverview }"
                     id="containerGridTDMAOVERFLOWRequestedText">
                     <div class="tdma_realtime_container_views_tdma_frameplan_column_overflow_text_container" :style="{
                        'grid-area': tdmaGenerator_row_column_overflow_text['grid-area'],
                        'color': tdmaGenerator_row_column_overflow_text['color']
                     }">
                        {{ tdmaGenerator_row_column_overflow_text.value }}
                     </div>

                  </div>
               </div>
            </div>

         </div>
         <div class="realtime_container_views_tdma_frameplan_container" v-if="framePlanviewActive">
            <div class="realtime_container_views_tdma_frameplan_label_container">
               <div class="tdma_labels_top">
                  <div v-for="label in labelsFrameplan" class="container_label">
                     <div :style="{ 'background': label.color }" class="circle_label"></div>
                     <div class="text_label_tdma">{{ label.name }}</div>
                  </div>
               </div>
            </div>
            <div class="realtime_container_views_tdma_frameplan_chart">
               <div class="realtime_container_views_tdma_frameplan_column_char_req">
                  <div class="realtime_container_views_tdma_frameplan_column_char_req_container"
                     :style="{ 'grid-template-rows': frameplanInroutesGenerator()['grid-template-rows'], 'grid-template-columns': frameplanInroutesGenerator()['grid-template-columns'] }">

                     <div v-for="(req, idxReq) in frameplansqueletorGeneratorChartRectangleAxis()"
                        :style="{ 'grid-area': req['grid-area'], 'border-left': req['border'] }">
                     </div>
                     <div v-for="(req, idxReq) in arrayCellColumn" id="arrayCellColumn" class="rectangle_border"
                        :style="{ 'grid-area': req['grid-area'], 'background-color': req['color'] }">
                     </div>
                     <div v-for="(req, idxReq) in arrayRectangleCell" class="rectangle_border"
                        :style="{ 'grid-area': req['grid-area'], 'background-color': req['color'] }">
                     </div>


                     <div v-for="(req, idxReq) in arrayStationRow" class="station_number"
                        :style="{ 'grid-area': `${req['grid-area']}`, 'border-right': '1px solid black' }">
                        
                        <span>{{ req['station-number'] }}</span>
                     </div>
                     <div v-for="(req, idxReq) in arrayInrouteCell" class="inroute_name"
                        :style="{ 'grid-area': `${req['grid-area']}` }">
                        Inroute {{ idxReq + 1 }}
                     </div>


                     <div v-for="(req, idxReq) in frameplansqueletorGeneratorChartRectangleAxisHorizontalLine()"
                        :style="{ 'grid-area': req['grid-area'], 'border-top': req['border'] }">
                     </div>
                     <div v-for="(req, idxReq) in frameplansqueletorGeneratorChartRectangleAxistinyLineunderNumber()"
                        :style="{ 'grid-area': req['grid-area'], 'border-left': req['border'] }">

                     </div>

                     <div v-for="(req, idxReq) in frameplansqueletorGeneratorChartRectangleAxisNumber()"
                        :style="{ 'grid-area': req['grid-area'], 'justify-content': req['position'] }"
                        class="realtime_rql" id="axis2">
                        {{ req['value-axis'] }}
                     </div>


                  </div>
               </div>
            </div>
         </div>
         <div class="realtime_container_views_tdma_station_container" v-if="stationviewActive">
            <div>
               <AgGridVue :modules="modules" :grid-options="gridOptions1" style="width: 100%; height: 100%;"
                  class="ag-theme-balham" :columnDefs="columnDefs" :rowData="rowDataTopRequested"
                  @gridReady="(params) => onGridReady(params, 'requested')" />
            </div>
            <div>
               <AgGridVue :modules="modules" :grid-options="gridOptions2" style="width: 100%; height: 100%;"
                  class="ag-theme-balham" :columnDefs="columnDefs" :rowData="rowDataTopAllocated"
                  @gridReady="(params) => onGridReady(params, 'allocated')" />
            </div>
         </div>
      </div>

   </div>
</template>

<script>
import { ClientSideRowModelModule } from '@ag-grid-community/client-side-row-model';
import { ColumnApi, GridApi } from '@ag-grid-community/core';
import { AgGridVue } from '@ag-grid-community/vue';
import { AgChartsVue } from "ag-charts-vue";
import * as agCharts from 'ag-charts-community';
import plug from './band_plug.json';
import final from './band_final.json';
/* import baseGrayChart from './data_sample.json'; */
import axios from 'axios';
import datarealtimeResponse from './realtimeResponse.json';
import band_finalResponse from './band_final.json';

export default {
   components: {
      'ag-charts-vue': AgChartsVue,
      AgGridVue,
   },
   data() {
      return {
         name_controller: '',
         intervalValueAxisCalGlobal: 0,
         tdmaGenerator_row_column: {},
         tdmaGenerator_row_column_overflow: {},
         tdmaGenerator_row_column_overflow_text: {},
         requestoverflow: 0,
         stringRealtime: "",
         dataLoaded: false,
         tdmaOverviewActive: false,
         framePlanviewActive: true,
         stationviewActive: false,
         stationFramePlan: [],
         stationData: [],
         stationDataOld: [],
         varStation: 'name,rx_errors,state,num_state,cur_slots,bw_rq,rx_number,rx_controller,rx_err_buf',
         controllerStruct: {
            name: '',
            total_channels: 0,
            slots_per_frame: 0,
            number_stations: 0,
            total_request: 0,
            total_reques_rt: 0,
            total_request_cir: 0,
            allocated: 0,
            total_slots: 0,
            idle: 0,
            free: 0,
            data: 0,
            acquisition: 0,
            zero: 0
         },
         stationfakedata: [
            [8, 'Unreachable', 2, 4, 0],
            [0, 'Unreachable', 2, 6, 0],
            [0, 'Unreachable', 2, 8, 0],
            [0, 'Unreachable', 6, 5, 5],
            [5, 'Unreachable', 1, 2, 0],
            [0, 'Unreachable', 2, 3, 0],
            [0, 'Unreachable', 2, 0, 0],
            [1, 'Unreachable', 2, 4, 0],
            [0, 'Unreachable', 2, 0, 0],
            [0, 'Unreachable', 2, 0, 0],
            [0, 'Unreachable', 2, 1, 0],
            [0, 'Unreachable', 2, 3, 0],
            [0, 'Unreachable', 2, 0, 0]
         ],

         showOverview: true,
         refreshIntervalId: null,

         showFramePlan: true,
         showStations: true,
         totalCountFrames: 0,
         options: null,
         Xlegends: ["Requested", "QoS requested"],
         XlegendsB: ["Allocated", "Frame Layout"],
         delay: 1000,
         statsButtons: this.$const('TDMA_STATS_COMMANDS'),
         timer: null,
         plug,
         final,
         chLength: 0,
         chAmount: 0,
         labelsTop: [
            { 'name': 'RT', 'color': '#e090f4' },
            { 'name': 'Request', 'color': '#3acbfc' },
            { 'name': 'CIR', 'color': '#f9d936' },
         ],
         labelsBottom: [
            { 'name': 'Allocated', 'color': '#5dab46' },
            { 'name': 'Free', 'color': '#eaeaea' },
            { 'name': 'Idle', 'color': '#b5fdff' },
            { 'name': 'Data', 'color': '#ff8c00' },
            { 'name': 'Acquisition', 'color': '#666' },
            { 'name': 'Zero', 'color': '#d33800' }
         ],
         numberAxis: [],
         labelsFrameplan: [
            { 'name': 'Down', 'color': 'gray' },
            { 'name': 'Idle', 'color': 'pink' },
            { 'name': 'CIR Data', 'color': 'lightgreen' },
            { 'name': 'PIR Data', 'color': 'darkgreen' },
            { 'name': 'CIR Realtime', 'color': 'lightblue' },
            { 'name': 'PIR Realtime', 'color': 'darkblue' },
            { 'name': 'Overload', 'color': 'red' },
            { 'name': 'CRC', 'color': 'orange' },
            { 'name': 'Base', 'color': 'white' }
         ],
         barsColors: {
            0: '#48C4ED',
            1: '#4F913C',
            2: '#CECECE',
            3: '#9CD9DB',
            4: '#FC9323',
            5: '#6D6D6D',
            6: 'red'
         },
         cellsColor: [
            { 'name': 'Down', 'color': 'black' },
            { 'name': 'Off', 'color': 'gray' },
            { 'name': 'Idle', 'color': 'cyan' },
            { 'name': 'CIR Data', 'color': 'lightgreen' },
            { 'name': 'PIR Data', 'color': 'darkgreen' },
            { 'name': 'CIR Realtime', 'color': 'black' },
            { 'name': 'PIR Realtime', 'color': 'black' },
            { 'name': 'Overload', 'color': 'red' },
            { 'name': 'No Name', 'color': 'white' },
            { 'name': 'Base', 'color': 'white' }
         ],
         cells: {},
         cellsToCalculate: '',
         repeatGridColumnFramePlan: 0,
         moduleDivisor: 0,
         inrouts: [],
         stations: [
            { 'id': 0, 'name': 'station 1', slots: 14 },
            { 'id': 1, 'name': 'station 2', slots: 13 },
            { 'id': 2, 'name': 'station 3', slots: 13 },
            { 'id': 2, 'name': 'station 3', slots: 13 },
            { 'id': 2, 'name': 'station 3', slots: 13 },


         ],

         intervalValueAxisOverflow: 17,
         baseintervalInitialOverflow: 1024,
         baseDivisorOverflow: 16,

         intervalValueAxis: 17,
         baseintervalInitial: 1024,
         baseDivisor: 64,

         posRectangleRequestedRT: 3,
         posRectangleRequested: 3,

         posRequestedCIR: 4,




         posRequestedALLOCATED: 7,
         posRectangleIDLE: 7,
         posRectangleFREE: 7,

         posRectangleDATA: 8,
         posRectangleACQUISITION: 8,
         posRectangleZERO: 8,
         arrayCellColumn: [],
         arrayInrouteCell: [],
         arrayStationRow: [],
         arrayRectangleCell: [],

         tdmasqueletorGeneratorChartRectangleRequestedRTValues: [],
         tdmasqueletorGeneratorChartRectangleRequestedValues: [],
         tdmasqueletorGeneratorChartRectangleRequestedCIRValues: [],
         tdmasqueletorGeneratorChartRectangleRequestedALLOCATEDValues: [],
         tdmasqueletorGeneratorChartRectangleIDLEValues: [],
         tdmasqueletorGeneratorChartRectangleFREEValues: [],
         tdmasqueletorGeneratorChartRectangleDATAValues: [],
         tdmasqueletorGeneratorChartRectangleACQUISITIONValues: [],
         tdmasqueletorGeneratorChartRectangleZEROValues: [],
         tdmasqueletorGeneratorChartRectangleOverflowValuesRequested: [],
         tdmasqueletorGeneratorChartRectangleOverflowValuesFrameLayout: [],

         RTValues: [],
         RequestedValues: [],

         CIRValues: [],

         ALLOCATEDValues: [],
         IDLEValues: [],
         FREEValues: [],

         DATAValues: [],
         ACQUISITIONValues: [],
         ZeroValues: [],
         OverflowValuesRequested: [],
         OverflowValuesFramelayout: [],
         indexRequestRow: 1,
         indexQoSRequest: 1,
         indexAllocated: 0,
         indexFrameLayout: 0,
         gridApi: {},
         gridOptions1: {
            deltaRowDataMode: true,
            rowSelection: 'multiple',
            suppressRowClickSelection: true
         },
         gridOptions2: {
            deltaRowDataMode: true,
            rowSelection: 'multiple',
            suppressRowClickSelection: true
         },
         gridColumnApi: {},
         modules: [ClientSideRowModelModule, GridApi, ColumnApi],
         columnDefs: [
            {
               field: "id",
               headerName: 'ID',
               sortable: true,
               resizable: true,
               filter: 'agNumberColumnFilter',
            },
            {
               field: "name",
               headerName: 'Name',
               sortable: true,
               resizable: true,
               filter: 'agTextColumnFilter',
               cellStyle: {
                  'text-transform': 'capitalize'
               },
            },
            {
               field: "inroute",
               headerName: 'Inroute',
               sortable: true,
               resizable: true,
               filter: 'agNumberColumnFilter',
            },
            {
               field: "rq_realtime",
               headerName: 'RQ realtime',
               sortable: true,
               resizable: true,
               filter: 'agNumberColumnFilter',
            },
            {
               field: "rq_total",
               headerName: 'RQ total',
               sortable: true,
               resizable: true,
               filter: 'agNumberColumnFilter',
            },
            {
               field: "allocated",
               headerName: 'Allocated',
               sortable: true,
               resizable: true,
               filter: 'agNumberColumnFilter',
            },
            {
               field: "cir",
               headerName: 'CIR',
               sortable: true,
               resizable: true,
               filter: 'agNumberColumnFilter',
            },
            {
               field: "rq_allocated",
               headerName: 'RQ Allocated',
               sortable: true,
               resizable: true,
               filter: 'agNumberColumnFilter',
            },
         ],
         rowDataTopRequested: [],
         rowDataTopAllocated: [],
         gridApiRequested: null,
         gridColumnApiRequested: null,
         gridApiAllocated: null,
         gridColumnApiAllocated: null,
         overflowtype1: false,
         overflowtype2: false,
         posLineunderNumber: 5,
         posHorizontalLine: 5,
         posAxisNumber: 6,
         gridTemplateRowsOverview: `minmax(1px, 0.03fr) minmax(1px, 0.01fr)  minmax(8px, 0.05fr) 0.05fr 0.02fr minmax(8px, 0.03fr) 0.05fr 0.05fr 0.01fr minmax(1px, 0.03fr)`,
         arrStationsCRCTime: []
      }

   },
   computed: {
   },
   methods: {
      text_row_type(pos, value, percentaje, who, distance) {
         var string_slots = 'slots';
         var string_divisor = '/';
         var string_percentaje = '%';
         if (distance >= 90) {
            return `${value} ${string_slots} ${string_divisor} ${percentaje} ${string_percentaje}`;
         } else if (distance >= 70) {
            return `${value} ${string_divisor} ${percentaje} ${string_percentaje}`;
         } else {
            return `${value}`;
         }
      },
      slot_percentaje_calculation(pos, value, who, posInit, posFinal) {
         var percentaje = Math.round((value * 100 / this.controllerStruct.total_slots) * 10) / 10;
         var string_result = '';

         var distance = posFinal - posInit;
         //         console.log(pos, who, posInit, posFinal, distance)

         //pos0 (total_request_rt & total_request_cir & allocated & Data)
         if (pos == 0) {
            string_result = this.text_row_type(pos, value, percentaje, who, distance);
         }
         //pos1 (total_request & Free & Acquisition)
         else if (pos == 1) {

            string_result = this.text_row_type(pos, value, percentaje, who, distance);
         }
         //pos2 (idle & zero)
         else {
            string_result = this.text_row_type(pos, value, percentaje, who, distance);
         }

         return string_result;
      },
      overflow_rt_req() {
         const condition1 = Math.round(((this.controllerStruct.total_reques_rt * 100) / this.controllerStruct.total_slots) * 10) / 10 > 100;
         const condition2 = Math.round(((this.controllerStruct.total_request * 100) / this.controllerStruct.total_slots) * 10) / 10 > 100;

         // Retornar true si cualquiera de las condiciones se cumple
         const result = condition1 || condition2;
         return result;
      },
      overflowcheck_total_request(overflow) {
         return (Math.round(((overflow * 100) / this.controllerStruct.total_slots) * 10) / 10 > 100) ? false : true;
      },
      border_calculation(value, index, posInicial, posFinal) {
         let border_obj = {
            'border': (this.overflowcheck_total_request(value)) ? 'none' : '1px solid red',
            'border_left': (this.overflowcheck_total_request(value)) ? 'none' : (index == posInicial) ? '1px solid red' : 'none',
            'border_right': (this.overflowcheck_total_request(value)) ? 'none' : (index == posFinal - 1) ? '1px solid red' : 'none'
         };
         return border_obj;
      },
      onGridReady(params, gridType) {
         if (gridType === "requested") {
            this.gridApiRequested = params.api;
            this.gridColumnApiRequested = params.columnApi;
            params.api.sizeColumnsToFit();
         } else if (gridType === "allocated") {
            this.gridApiAllocated = params.api;
            this.gridColumnApiAllocated = params.columnApi;
            params.api.sizeColumnsToFit();
         }
      },
      frameplanInroutesGenerator() {
         let objColumnConfig = null;
         objColumnConfig = {
            'grid-template-columns': `repeat(${this.repeatGridColumnFramePlan}, minmax(0, 1fr))`,
            /*  'grid-template-rows': `repeat(${(this.inrouts.length * 4) + 4}, 0.04fr)`, */
            //
            'grid-template-rows': `0.02fr 0.02fr repeat(${(this.inrouts.length * 4)}, 0.04fr) 0.02fr`,
         }
         return objColumnConfig
      },
      tdmaGenerator() {
         let objColumnConfig = null;
         let limit_axis = this.baseintervalInitial;

         objColumnConfig = {
            'grid-template-columns': `repeat(${limit_axis}, minmax(0, 1fr))`,
            'grid-template-rows': this.gridTemplateRowsOverview,
         }
         this.tdmaGenerator_row_column = objColumnConfig;
         return objColumnConfig;
      },
      tdmaGeneratorOverflow() {
         let limit_axis = this.baseintervalInitialOverflow;
         let objColumnConfig = {
            'grid-template-columns': `repeat(${limit_axis}, minmax(0, 1fr))`,
            'grid-template-rows': this.gridTemplateRowsOverview,
         }
         this.tdmaGenerator_row_column_overflow = objColumnConfig;
         return objColumnConfig;
      },
      tdmaGeneratorOverflowText() {
         let objColumnConfig = {};
         let module = this.mapValueToGrid('request_rt', this.controllerStruct.total_reques_rt, this.controllerStruct.total_slots, this.baseintervalInitial);
         module = module + Math.round(module * 0.785)

         let percentajeValue = this.slot_percentaje_calculation(1, this.controllerStruct.total_request, 'total_request', 0, 200);
         if (this.overflowtype1) {
            module = (module == 0) ? module + 1 : module;
            objColumnConfig = {
               'grid-area': `3/ ${module} / 3 / 2048`,
               'value': percentajeValue,
               'color': (this.overflowcheck_total_request(this.controllerStruct.total_request)) ? 'black' : 'red',

            }
         }

         this.tdmaGenerator_row_column_overflow_text = objColumnConfig;
         return objColumnConfig;
      },
      mapValueToGridOverflow(type, value, new_max, limit_axis) {
         return Math.ceil(value * (limit_axis / new_max)) + 1;
      },
      //his.generateAxis(this.controllerStruct.slots_per_frame, this.intervalValueAxis);
      generateAxis(new_max, num_points) {
         const step = new_max / (num_points - 1); // Intervalo uniforme
         return Array.from({ length: num_points }, (_, i) => Math.round(i * step));
      },
      mapValueToGrid(type, value, new_max, limit_axis) {
         if (value === 0) return 0;
         //*total request rt & total request cir & Allocated & Data
         if (type == 'request_rt' || type === 'request_cir' || type === 'allocated' || type === 'data') {
            //value1<totalslots
            if (value < this.controllerStruct.total_slots) {

               return Math.round(value * (limit_axis / new_max));
            }
            //value1==totalslots
            else if (value == this.controllerStruct.total_slots) {

               return Math.ceil(value * (limit_axis / new_max)) + 1;
            }
            //value1>totalslots
            else if (value > this.controllerStruct.total_slots) {
               return Math.ceil(this.controllerStruct.total_slots * (limit_axis / new_max)) + 1;
            }
         }

         //*Free & Acquisition
         else if (type == 'free' || type == 'acquisition') {
            if (type == 'acquisition') {

               if (value == 0) {
                  this.overflowtype2 = false;
                  return 0;
               }

               if (this.controllerStruct.data >= this.controllerStruct.total_slots) {
                  this.overflowtype2 = true;
                  value = 0;
               } else {
                  this.overflowtype2 = false;
               }
               /*   console.log('acquisition',this.controllerStruct.acquisition)
                 console.log('value:', value)
                 console.log('data:',this.controllerStruct.data )
                 console.log('overflowtype2:',this.overflowtype2 ) */

            }
            return Math.ceil(value * (limit_axis / new_max));
         }
         //value1+value2<totalslots 
         //value1+value2==totalslots
         //value>totalslots
         //*Requested & Idle & zero
         else if (type == 'requested' || type == 'idle' || type == 'zero') {
            if (type == 'requested') {
               //value1+value2<totalslots
               if (this.controllerStruct.total_reques_rt + this.controllerStruct.total_request < this.controllerStruct.total_slots) {
                  this.overflowtype1 = false;
                  return this.indexRequestRow + Math.ceil(value * (limit_axis / new_max)); // Opción 2
               }
               //value1+value2==totalslots
               else if (this.controllerStruct.total_reques_rt + this.controllerStruct.total_request == this.controllerStruct.total_slots) {

                  var resto = this.controllerStruct.total_slots - this.controllerStruct.total_reques_rt;
                  return this.indexRequestRow + Math.ceil(resto * (limit_axis / new_max)) + 1;
               }
               //value1+value2>totalslots
               else if (this.controllerStruct.total_reques_rt + this.controllerStruct.total_request > this.controllerStruct.total_slots) {
                  if (this.controllerStruct.total_reques_rt >= this.controllerStruct.total_request) {
                     let restValue = this.controllerStruct.total_slots + (this.controllerStruct.total_slots - this.controllerStruct.total_reques_rt);

                     this.overflowtype1 = true;
                     return Math.ceil(restValue * (limit_axis / new_max)) + 1;
                  } else if (this.controllerStruct.total_reques_rt == this.controllerStruct.total_slots) {
                     this.overflowtype1 = true;
                     return 0;
                  }
                  else {
                     this.overflowtype1 = true;
                     return Math.round(this.controllerStruct.total_slots * (limit_axis / new_max));
                  }
               }


            }
         }
         //value1+value2+value3<totalslots 
         //value1+value2+value3==totalslots
         //value1+value2+value3>totalslots

         return Math.round(value * (limit_axis / new_max));
      },
      findFirstAndLast(arr, x) {
         let n = arr.length;
         let first = -1, last = -1;
         for (let i = 0; i < n; i++) {
            if (!arr[i])
               continue;
            if (x != arr[i].station)
               continue;
            if (first == -1)
               first = i;
            last = i;
         }
         if (first != -1) {
            //console.log(`${x} has positions from ${first} to ${last}`);
            return [first, last]
         }
         else
            return null

      },
      stationInInroute(array) {
         var categories = [];
         for (var i = 0, len = array.length; i < len; i++) {
            if (!array[i])
               continue;
            if (categories.indexOf(array[i].station) > -1) {
            }
            else {
               categories.push(array[i].station);
            }
         }

         return categories
      },
      formatStationNumber(number) {
         const num = Number(number);
         // Ajusta estos umbrales según necesites
         if (num >= 1000000) {
            return (num / 1000000).toFixed(1) + 'M';
         } else if (num >= 1000) {
            return (num / 1000).toFixed(1) + 'K';
         }
         return num.toString();
      },
      frameplansqueletorGeneratorChartRectangle() {
         let arrayColumn = [];
         let arrayStationNumber = [];
         let columnCell = 5;
         let numberOfColumns = this.controllerStruct.total_channels;
         let cellsPerColumn = this.controllerStruct.slots_per_frame;
         let totalCells = cellsPerColumn * numberOfColumns; // Dynamic total cells for initial setup
         let grid = Array(totalCells).fill(null);
         let currentCell = 0;
         let results = [];
         let objColumnConfigStation = null;
         let rowNumberStation = 6;
         this.stationData.forEach((station, index) => {
            if (station.allocated === 0) {
               return;
            }
            if (currentCell + station.allocated > totalCells) {
               return;
            }

            let stationFirstCell = currentCell;
            let stationLastCell = currentCell + station.allocated - 1;

            station.colors.forEach(colorGroup => {
               let { amount, color, name } = colorGroup;
               for (let i = 0; i < amount; i++) {
                  if (currentCell >= totalCells) {
                     return;
                  }

                  grid[currentCell] = {
                     station: index + 1,
                     color: color
                  };

                  let resultObj = {
                     station: index + 1,
                     column: Math.floor(currentCell / cellsPerColumn) + 1,
                     row: (currentCell % cellsPerColumn) + 1,
                     cell: currentCell + 1,
                     color: color,
                     'grid-area': `${columnCell + (4 * Math.floor(currentCell / cellsPerColumn))} / ${(currentCell % cellsPerColumn) + 1} / ${columnCell + (4 * Math.floor(currentCell / cellsPerColumn))} /${(currentCell % cellsPerColumn) + 1}`,
                  };
                  results.push(resultObj);
                  currentCell++;
               }
            });
         });

         const gridByColumns = [];
         for (let i = 0; i < numberOfColumns; i++) {
            gridByColumns.push(grid.slice(i * cellsPerColumn, (i + 1) * cellsPerColumn));
         }
         gridByColumns.forEach((element, indexInroute) => {
            let rowNumberStationInroute = rowNumberStation + (indexInroute * 4);
            let temporalStations = this.stationInInroute(element);
            temporalStations.forEach(stationInInroute => {
               this.stationData = this.stationData.map(station => {
                  if (station.id === stationInInroute) {
                     station.inroute.push(indexInroute + 1);
                     return station;
                  }
                  return station;
               });
               let posStationLine = this.findFirstAndLast(element, stationInInroute);
               if (posStationLine) {
                  objColumnConfigStation = {
                     'grid-area': `${rowNumberStationInroute} / ${posStationLine[0] + 1} / ${rowNumberStationInroute} /${posStationLine[1] + 2}`,
                     'station-number': `${stationInInroute}`
                  };
                  arrayStationNumber.push(objColumnConfigStation);
               }
            });
         });

         this.arrayStationRow = arrayStationNumber;
         this.arrayRectangleCell = results;
         return arrayColumn;
      },
      tdmasqueletorGeneratorChartRectangleRequestedRT() {
         let arrayColumn = []
         let arrayValuesColumn = []
         let module = this.mapValueToGrid('request_rt', this.controllerStruct.total_reques_rt, this.controllerStruct.total_slots, this.baseintervalInitial);
         module = (module == 0) ? 0 : this.indexRequestRow + module;
         let row = this.posRectangleRequestedRT
         let objColumnConfigValue = null;
         for (let index = this.indexRequestRow; index < module; index++) {
            let objColumnConfig = null;
            let border = this.border_calculation(this.controllerStruct.total_reques_rt, index, 0, module);
            objColumnConfig = {
               'grid-area': `${row} / ${index} / ${row} /${index}`,
               'color': '#e090f4',
               'border-left': border.border_left,
               'border-right': border.border_right,
               'border': border.border
            }
            this.indexRequestRow += 1
            arrayColumn.push(objColumnConfig)
         }
         if (this.controllerStruct.total_reques_rt != 0) {
            objColumnConfigValue = {
               'grid-area': `${row} / ${1} / ${row} /${module}`,
               'value': this.slot_percentaje_calculation(0, this.controllerStruct.total_reques_rt, 'total_reques_rt', 0, module),
               'color': (this.overflowcheck_total_request(this.controllerStruct.total_reques_rt)) ? 'black' : 'red',
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }
         this.RTValues = arrayValuesColumn
         this.tdmasqueletorGeneratorChartRectangleRequestedRTValues = arrayColumn

         return arrayColumn
      },
      tdmasqueletorGeneratorChartRectangleRequestedCIR() {
         let arrayColumn = []
         let arrayValuesColumn = []
         let objColumnConfigValue = null;
         let module = this.mapValueToGrid('request_cir', this.controllerStruct.total_request_cir, this.controllerStruct.total_slots, this.baseintervalInitial);
         module = (module == 0) ? 0 : this.indexQoSRequest + module;
         let row = this.posRequestedCIR
         for (let index = this.indexQoSRequest; index < module; index++) {
            let objColumnConfig = null;
            let border = this.border_calculation(this.controllerStruct.total_request_cir, index, 0, module);
            objColumnConfig = {
               'grid-area': `${row} / ${index} / ${row} /${index}`,
               'color': '#f9d936',
               'border-left': border.border_left,
               'border-right': border.border_right,
               'border': border.border
            }
            arrayColumn.push(objColumnConfig)
         }
         if (this.controllerStruct.total_request_cir != 0) {
            let percentaje = (this.controllerStruct.total_request_cir * 100) / this.controllerStruct.total_slots
            percentaje = Math.round(percentaje * 10) / 10;
            objColumnConfigValue = {
               'grid-area': `${row} / ${1} / ${row} /${module}`,
               'value': this.slot_percentaje_calculation(0, this.controllerStruct.total_request_cir, 'total_request_cir', 0, module),
               'color': (this.overflowcheck_total_request(this.controllerStruct.total_request_cir)) ? 'black' : 'red',
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }
         this.CIRValues = arrayValuesColumn
         this.tdmasqueletorGeneratorChartRectangleRequestedCIRValues = arrayColumn
         return arrayColumn
      },
      tdmasqueletorGeneratorChartRectangleRequestedALLOCATED() {
         let arrayColumn = []
         let arrayValuesColumn = []
         let objColumnConfigValue = null;
         let module = this.mapValueToGrid('allocated', this.controllerStruct.allocated, this.controllerStruct.total_slots, this.baseintervalInitial);
         module = (module == 0) ? 0 : this.indexAllocated + module;

         let row = this.posRequestedALLOCATED
         for (let index = this.indexAllocated; index < module; index++) {
            let objColumnConfig = null;
            objColumnConfig = {
               'grid-area': `${row} / ${index} / ${row} /${index}`,
               'color': '#5dab46'
            }
            this.indexAllocated += 1
            arrayColumn.push(objColumnConfig)
         }
         if (this.controllerStruct.allocated != 0) {
            let percentaje = (this.controllerStruct.allocated * 100) / this.controllerStruct.total_slots
            percentaje = Math.round(percentaje * 10) / 10;

            objColumnConfigValue = {
               'grid-area': `${row} / ${1} / ${row} /${module}`,
               'color': '#d33800',
               'value': this.slot_percentaje_calculation(0, this.controllerStruct.allocated, 'allocated', 0, module),
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }

         this.ALLOCATEDValues = arrayValuesColumn
         this.tdmasqueletorGeneratorChartRectangleRequestedALLOCATEDValues = arrayColumn
         return arrayColumn
      },
      tdmasqueletorGeneratorChartRectangleDATA() {
         let arrayColumn = []
         let arrayValuesColumn = []
         let objColumnConfigValue = null;
         let module = this.mapValueToGrid('data', this.controllerStruct.data, this.controllerStruct.total_slots, this.baseintervalInitial);
         module = (module == 0) ? 0 : module + 1;
         let row = this.posRectangleDATA
         for (let index = this.indexFrameLayout; index < module; index++) {
            let objColumnConfig = null;
            objColumnConfig = {
               'grid-area': `${row} / ${index} / ${row} /${index}`,
               'color': '#ff8c00'
            }
            this.indexFrameLayout += 1
            arrayColumn.push(objColumnConfig)
         }
         if (this.controllerStruct.data != 0) {
            objColumnConfigValue = {
               'grid-area': `${row} / ${1} / ${row} /${module}`,
               'color': '#d33800',
               'value': this.slot_percentaje_calculation(0, this.controllerStruct.data, 'data', 0, module),
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }

         this.DATAValues = arrayValuesColumn
         this.tdmasqueletorGeneratorChartRectangleDATAValues = arrayColumn
         return arrayColumn
      },
      tdmasqueletorGeneratorChartRectangleRequested() {
         let arrayColumn = []
         let arrayValuesColumn = []
         let objColumnConfigValue = null;
         let posInicial = this.indexRequestRow;
         let module = this.mapValueToGrid('requested', this.controllerStruct.total_request, this.controllerStruct.total_slots, this.baseintervalInitial);
         let row = this.posRectangleRequested;
         module = (module == 0) ? 0 : module + 1;
         for (let index = posInicial; index <= module; index++) {
            let objColumnConfig = null;
            let border = this.border_calculation(this.controllerStruct.total_request, index, posInicial, module);
            objColumnConfig = {
               'grid-area': `${row} / ${index} / ${row} /${index}`,
               'color': '#3acbfc',
               'border-left': border.border_left,
               'border-right': (this.overflowtype1 && !this.overflowcheck_total_request(this.controllerStruct.total_request)) ? 'none ' : border.border_right,
               'border': border.border
            }
            this.indexRequestRow += 1
            arrayColumn.push(objColumnConfig)
         }
         let percentajeValue = this.slot_percentaje_calculation(1, this.controllerStruct.total_request, 'total_request', posInicial, module);
         //mapValueToGrid
         percentajeValue = (!this.overflowtype1) ? percentajeValue : '';
         if (this.controllerStruct.total_request != 0) {
            objColumnConfigValue = {
               'grid-area': `${row} / ${posInicial} / ${row} /${module}`,
               'value': percentajeValue,
               'color': (this.overflowcheck_total_request(this.controllerStruct.total_request)) ? 'black' : 'red',
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }
         this.RequestedValues = arrayValuesColumn;
         this.tdmasqueletorGeneratorChartRectangleRequestedValues = arrayColumn;

         return arrayColumn
      },
      tdmasqueletorGeneratorChartRectangleACQUISITION() {
         let arrayColumn = []
         let arrayValuesColumn = []
         let objColumnConfigValue = null;
         let posInicial = this.indexFrameLayout
         let module = this.mapValueToGrid('acquisition', this.controllerStruct.acquisition, this.controllerStruct.total_slots, this.baseintervalInitial);
         module = (module == 0) ? 0 : posInicial + module;

         let row = this.posRectangleACQUISITION
         for (let index = posInicial; index < module; index++) {
            let objColumnConfig = null;
            objColumnConfig = {
               'grid-area': `${row} / ${index} / ${row} /${index}`,
               'color': '#666'
            }
            this.indexFrameLayout += 1
            arrayColumn.push(objColumnConfig)
         }
         if (module != 0) {
            objColumnConfigValue = {
               'grid-area': `${row} / ${posInicial} / ${row} /${module}`,
               'color': '#d33800',
               'value': this.slot_percentaje_calculation(1, this.controllerStruct.acquisition, 'acquisition', posInicial, module),
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }
         this.ACQUISITIONValues = arrayValuesColumn;
         this.tdmasqueletorGeneratorChartRectangleACQUISITIONValues = arrayColumn;
         return arrayColumn;
      },
      tdmasqueletorGeneratorChartRectangleIDLE() {
         let arrayColumn = []
         let arrayValuesColumn = []
         let objColumnConfigValue = null;
         let posInicial = this.indexAllocated
         let module = this.mapValueToGrid('idle', this.controllerStruct.idle, this.controllerStruct.total_slots, this.baseintervalInitial);
         module = (module == 0) ? 0 : posInicial + module;
         let row = this.posRectangleIDLE
         for (let index = this.indexAllocated; index < module; index++) {
            let objColumnConfig = null;
            objColumnConfig = {
               'grid-area': `${row} / ${index} / ${row} /${index}`,
               'color': '#b5fdff'
            }
            this.indexAllocated += 1
            arrayColumn.push(objColumnConfig)
         }
         if (this.controllerStruct.idle != 0) {
            objColumnConfigValue = {
               'grid-area': `${row} / ${posInicial} / ${row} /${module}`,
               'color': '#d33800',
               'value': this.slot_percentaje_calculation(2, this.controllerStruct.idle, 'idle', posInicial, module),
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }

         this.IDLEValues = arrayValuesColumn
         this.tdmasqueletorGeneratorChartRectangleIDLEValues = arrayColumn
         return arrayColumn
      },

      tdmasqueletorGeneratorChartRectangleFREE() {
         let arrayColumn = []
         let arrayValuesColumn = []
         let objColumnConfigValue = null;
         let posInicial = this.indexAllocated
         let module = this.mapValueToGrid('free', this.controllerStruct.free, this.controllerStruct.total_slots, this.baseintervalInitial);
         module = (module == 0) ? 0 : posInicial + module;
         let row = this.posRectangleFREE
         for (let index = this.indexAllocated; index < module; index++) {
            let objColumnConfig = null;
            objColumnConfig = {
               'grid-area': `${row} / ${index} / ${row} /${index}`,
               'color': '#eaeaea'
            }
            this.indexAllocated += 1
            arrayColumn.push(objColumnConfig)
         }
         if (this.controllerStruct.free != 0) {
            objColumnConfigValue = {
               'grid-area': `${row} / ${posInicial} / ${row} /${module}`,
               'color': '#d33800',
               'value': this.slot_percentaje_calculation(1, this.controllerStruct.free, 'free', posInicial, module),
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }
         this.FREEValues = arrayValuesColumn
         this.tdmasqueletorGeneratorChartRectangleFREEValues = arrayColumn
         return arrayColumn
      },
      tdmasqueletorGeneratorChartRectangleZERO() {
         let arrayColumn = [];
         let arrayValuesColumn = [];
         let posInicial = this.indexFrameLayout;
         let module = this.mapValueToGrid('zero', this.controllerStruct.zero, this.controllerStruct.total_slots, this.baseintervalInitial);
         module = (module == 0) ? 0 : posInicial + module;

         let row = this.posRectangleZERO;

         let objColumnConfigValue = null;
         for (let index = this.indexFrameLayout; index < module; index++) {
            let objColumnConfig = null;
            objColumnConfig = {
               'grid-area': `${row} / ${index} / ${row} /${index}`,
               'color': '#d33800'
            }
            arrayColumn.push(objColumnConfig)
         }
         if (this.controllerStruct.zero > 0) {
            objColumnConfigValue = {
               'grid-area': `${row} / ${posInicial} / ${row} /${module}`,
               'color': '#d33800',
               'value': this.slot_percentaje_calculation(2, this.controllerStruct.zero, 'zero', posInicial, module),
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }

         this.ZeroValues = arrayValuesColumn;
         this.tdmasqueletorGeneratorChartRectangleZEROValues = arrayColumn;
         return arrayColumn;
      },
      tdmasqueletorGeneratorChartRectangleOverflowRequested() {
         let arrayColumn = [];
         let arrayValuesColumn = [];
         let posInicial = 1;
         let overflowValue = this.baseintervalInitialOverflow;
         let newMax = this.baseintervalInitialOverflow

         if (overflowValue >= this.baseintervalInitialOverflow) {
            newMax = overflowValue;
         }
         let module = this.mapValueToGridOverflow('requested', overflowValue, newMax, this.baseintervalInitialOverflow);

         if (this.controllerStruct.total_request == 0) this.overflowtype1 = false;
         let row = this.posRectangleRequested;
         let objColumnConfigValue = null;
         if (this.overflowtype1) {
            for (let index = posInicial; index < module; index++) {
               let objColumnConfig = null;

               let border = this.border_calculation(this.controllerStruct.total_request, index, posInicial, module);
               objColumnConfig = {
                  'grid-area': `${row} / ${index} / ${row} /${index}`,
                  'color': '#3acbfc',
                  'border-right': border.border_right,
                  'border': border.border
               }
               arrayColumn.push(objColumnConfig)
            }
         }

         if (this.overflowtype1) {
            objColumnConfigValue = {
               'grid-area': `${row} / ${1} / ${row} /${this.baseintervalInitialOverflow}`,
               'value': '',
               'color': (this.overflowcheck_total_request(this.controllerStruct.total_request)) ? 'black' : 'red',
            }
            arrayValuesColumn.push(objColumnConfigValue)
         }

         this.OverflowValuesRequested = arrayValuesColumn;
         this.tdmasqueletorGeneratorChartRectangleOverflowValuesRequested = arrayColumn;
         return arrayColumn;
      },
      tdmasqueletorGeneratorChartRectangleOverflowFrameLayout() {
         let arrayColumn = [];
         let arrayValuesColumn = [];
         let posInicial = 1;
         let module = this.mapValueToGridOverflow('frame-layout', this.baseintervalInitialOverflow, this.baseintervalInitialOverflow, this.baseintervalInitialOverflow);
         module = (module == 0) ? 0 : posInicial + module;

         if (this.controllerStruct.acquisition == 0) this.overflowtype2 = false;

         let row = this.posRectangleDATA;

         let objColumnConfigValue = null;
         if (this.overflowtype2) {
            for (let index = posInicial; index < module; index++) {
               let objColumnConfig = null;
               objColumnConfig = {
                  'grid-area': `${row} / ${index} / ${row} /${index}`,
                  'color': '#666'
               }
               arrayColumn.push(objColumnConfig)
            }
            if (true) {
               objColumnConfigValue = {
                  'grid-area': `${row} / ${posInicial} / ${row} /${module}`,
                  'value': this.slot_percentaje_calculation(1, this.controllerStruct.acquisition, 'acquisition', posInicial, module),
               }
               arrayValuesColumn.push(objColumnConfigValue)
            }
         }

         this.OverflowValuesFramelayout = arrayValuesColumn;
         this.tdmasqueletorGeneratorChartRectangleOverflowValuesFrameLayout = arrayColumn;
         return arrayColumn;
      },
      frameplansqueletorGeneratorChartInrouteName() {
         let objColumnConfig = null;
         let arrayColumn = []

         let row = 4
         for (let index = 0; index < this.inrouts.length; index++) {

            let objColumnConfig = null;
            objColumnConfig = {
               'grid-area': `${row} / 1 / ${row} /15`,
            }
            arrayColumn.push(objColumnConfig)


            row += 4
         }
         this.arrayInrouteCell = arrayColumn
         return arrayColumn
      },

      frameplansqueletorGeneratorChartRectangleAxistinyLineunderNumber() {
         let arrayColumn = []
         let module = this.repeatGridColumnFramePlan;
         let divisor = this.moduleDivisor;
         let border = '1px solid #80808078';
         let colorIndex = 0;
         for (let index = 0; index < module; index++) {
            let objColumnConfig = null;
            /// grid-area: 1 / 5 / 1 / 6;
            objColumnConfig = {
               'grid-area': `2 / ${index + 1} / 2 /${index + 1}`,
               'border-right': border,
               'value-axis': module,
               'color': this.barsColors[2]
            }
            if (index % divisor == 0) {
               objColumnConfig.color = this.barsColors[6]
               objColumnConfig['border'] = border
            } else {
               objColumnConfig.color = this.barsColors[5]
               objColumnConfig['border'] = 'none'
            }
            arrayColumn.push(objColumnConfig)
         }
         return arrayColumn
      },
      frameplansqueletorGeneratorChartRectangleAxisHorizontalLine() {
         let arrayColumn = []
         let module = this.repeatGridColumnFramePlan;
         let divisor = this.moduleDivisor;
         let border = '1px solid #80808078';
         let colorIndex = 0;
         for (let index = 0; index < module; index++) {
            let objColumnConfig = null;
            /// grid-area: 1 / 5 / 1 / 6;
            objColumnConfig = {
               'grid-area': `3 / ${index + 1} / 3 /${index + 1}`,
               'border': border,
               'value-axis': module,
               'color': this.barsColors[2]
            }
            arrayColumn.push(objColumnConfig)
         }
         return arrayColumn
      },
      frameplansqueletorGeneratorChartRectangleAxis() {
         let arrayColumn = []
         let module = this.repeatGridColumnFramePlan;
         let divisor = this.moduleDivisor;
         let border = '1px solid #80808078';
         let colorIndex = 0;
         for (let index = 0; index < module; index++) {
            let objColumnConfig = null;
            /// grid-area: 1 / 5 / 1 / 6;
            objColumnConfig = {
               'grid-area': `3 / ${index + 1} / ${(this.inrouts.length * 7)} /${index + 1}`,
               'border-right': border,
               'value-axis': module,
               'color': this.barsColors[2]
            }
            if (index % divisor == 0) {
               objColumnConfig.color = this.barsColors[6]
               objColumnConfig['border'] = border
            } else {
               objColumnConfig.color = this.barsColors[5]
               objColumnConfig['border'] = 'none'
            }
            arrayColumn.push(objColumnConfig)
         }
         return arrayColumn
      },
      getFactorsList(num) {
         const factors = [];
         for (let i = 1; i <= Math.sqrt(num); i++) {
            if (num % i === 0) {
               factors.push(i);
               if (i !== num / i) {
                  factors.push(num / i);
               }
            }
         }
         return factors.sort((a, b) => a - b);
      },
      getFactors(total_slots) {
         const steps = (total_slots / 16) - 1;
         let intervalValueAxisCal;

         if (!Number.isInteger(steps)) {
            const factors = this.getFactorsList(total_slots);

            if (total_slots === 244) {
               intervalValueAxisCal = 61;
            } else {
               const sqrt = Math.sqrt(total_slots);
               intervalValueAxisCal = factors.reduce((prev, curr) =>
                  Math.abs(curr - sqrt) < Math.abs(prev - sqrt) ? curr : prev
               );
            }
         } else {
            if (steps < 4) {
               intervalValueAxisCal = 2;
            }
         }

         return intervalValueAxisCal;
      },
      tdmaesqueletorGeneratorChartRectangleAxisHorizontalLine() {
         let arrayColumn = []
         let module = this.baseintervalInitial;
         for (let index = 0; index < module; index++) {
            let objColumnConfig = null;
            /// grid-area: 1 / 5 / 1 / 6;
            let color = '#80808078';
            objColumnConfig = {
               'grid-area': `${this.posHorizontalLine} / ${index + 1} / ${this.posHorizontalLine} /${index + 1}`,
               'border': `1px solid ${color}`,
               'value-axis': module,
               'color': this.barsColors[2]
            }
            arrayColumn.push(objColumnConfig)
         }
         return arrayColumn
      },
      tdmaesqueletorGeneratorChartRectangleAxistinyLineunderNumber() {
         let arrayColumn = []
         let module = this.baseintervalInitial + 1;
         let divisor = this.baseDivisor;
         let border = '1px solid #80808078';
         let colorIndex = 0;
         for (let index = 0; index < module; index++) {
            let objColumnConfig = null;
            /// grid-area: 1 / 5 / 1 / 6;
            objColumnConfig = {
               'grid-area': `${this.posLineunderNumber} / ${index + 1} / ${this.posLineunderNumber} /${index + 1}`,
               'border-right': border,
               'value-axis': module,
               'color': this.barsColors[2]
            }
            if (index % divisor == 0) {
               //console.log('if1')
               objColumnConfig.color = this.barsColors[6]
               objColumnConfig['border'] = border
            } else {
               //console.log('if2')
               objColumnConfig.color = this.barsColors[5]
               objColumnConfig['border'] = 'none'
            }
            arrayColumn.push(objColumnConfig)
         }
         return arrayColumn
      },
      tdmaesqueletorGeneratorChartRectangleAxisNumber() {
         let arrayColumn = []
         let module = this.baseintervalInitial / this.baseDivisor;
         let divisor = this.baseDivisor;
         let border = '1px solid blue';
         let totalFrames = this.controllerStruct.total_slots
         let objColumnConfig = null;
         objColumnConfig = {
            'grid-area': `${this.posAxisNumber}/ ${1} / ${this.posAxisNumber}/${1}`,
            'border-right': border,
            'value-axis': 0,
            'color': this.barsColors[2],
            'border': border,
            'position': 'flex-start'
         }
         arrayColumn.push(objColumnConfig)
         const axis = this.generateAxis(this.controllerStruct.total_slots, this.intervalValueAxis);
         for (let index = 1; index < module; index++) {
            objColumnConfig = {
               'grid-area': `${this.posAxisNumber}/ ${index * divisor} / ${this.posAxisNumber}/${(index * divisor) + 2}`,
               'border-right': border,
               'value-axis': axis[index],
               'color': this.barsColors[2],
               'border': border,
               'position': 'center'
            }
            arrayColumn.push(objColumnConfig)
         }
         objColumnConfig = {
            'grid-area': `${this.posAxisNumber}/ ${this.baseintervalInitial} / ${this.posAxisNumber}/${this.baseintervalInitial}`,
            'border-right': border,
            'value-axis': this.controllerStruct.total_slots,
            'color': this.barsColors[2],
            'border': border,
            'position': 'flex-end'
         }
         arrayColumn.push(objColumnConfig)
         this.numberAxis = arrayColumn


         return arrayColumn
      },
      frameplansqueletorGeneratorChartRectangleAxisNumber() {
         let arrayColumn = []
         let module = this.repeatGridColumnFramePlan / this.moduleDivisor;
         let divisor = this.moduleDivisor;

         let border = '1px solid blue';
         let colorIndex = 0;
         for (let index = 0; index <= module; index++) {
            let objColumnConfig = null;
            objColumnConfig = {
               'grid-area': `1 / ${index * divisor} / 1 /${(index * divisor) + 2}`,
               'border-right': border,
               'value-axis': index * divisor,
               'color': this.barsColors[2]
            }
            if (index == 0) {
               objColumnConfig['position'] = 'flex-start';
            } else if (index == module) {
               objColumnConfig['position'] = 'flex-end';
            } else {
               objColumnConfig['position'] = 'center';
            }
            if (index % divisor == 0) {
               objColumnConfig.color = this.barsColors[6]
               objColumnConfig['border'] = border
            } else {
               objColumnConfig.color = this.barsColors[5]
               objColumnConfig['border'] = 'none'
            }
            arrayColumn.push(objColumnConfig)
         }
         return arrayColumn
      },

      changeView(value) {
         if (value == 'Overview') {
            this.tdmaOverviewActive = true;
            this.framePlanviewActive = false;
            this.stationviewActive = false;
         }
         if (value == 'FramePlan') {
            this.framePlanviewActive = true;
            this.stationviewActive = false;
            this.tdmaOverviewActive = false;
         }
         if (value == 'Stations') {
            this.stationviewActive = true;
            this.tdmaOverviewActive = false;
            this.framePlanviewActive = false;
         }
      },
      colorCell(indexCellStationForColor, color) {
         let obj = color.find(o => o.amount[0] <= indexCellStationForColor && indexCellStationForColor <= o.amount[1]);

         if (obj) {
            return obj.color
         }
         return 'red'
      },
      async executeTask(item) {
         try {
            // Await both promises
            const urlController = `${this.$const('CORE_SRV')}/realtime/get/controller${this.$route.params.pathMatch}`;
            const urlStations = `${this.$const('CORE_SRV')}/station/list/controller${this.$route.params.pathMatch}`;
            const dataController = await axios.post(urlController, { command: 'ge 96' })
            const dataControllerCRC = await axios.post(urlController, { command: 'sh st' })
            const dataStations = await axios.post(urlStations, { vars: this.varStation })
            this.inrouts = []
            this.dataLoaded = false;
            this.indexRequestRow = 1;
            this.indexQoSRequest = 1;
            this.indexAllocated = 1;
            this.indexFrameLayout = 1;
            this.stationDataOld = this.stationData;
            this.stationData = []

            let pathList = [];

            for (let item of dataController.data.path) {
               let url = item.split('_|_')[0];
               let name = item.split('_|_')[1];
               pathList.push({
                  'url': url,
                  'name': name
               })
            };

            this.$emit('bread-crumbs', pathList);
            let processingPath;
            var preparingPaths = [];
            processingPath = pathList.forEach(element => {
               let pathArr = element.url.split("/");
               let sortedPaths = [];
               sortedPaths = pathArr[3].split(",");
               let name = [];
               let id = [];

               if (sortedPaths.length === 1) {
                  name = sortedPaths[0].split("=")[0];
                  id = sortedPaths[0].split("=")[1];
                  let pathObj = {
                     'name': name,
                     "id": id
                  }
                  preparingPaths.push(pathObj);
               }
            });
            this.$emit('path-array', preparingPaths);
            if (dataController.data.reply != "No reply") {
               this.parseRealTimeController(dataController.data.reply);
               this.parseStation(dataController.data.reply, dataStations.data.reply, dataControllerCRC.data.reply);
               this.tdmaGenerator();
               this.mapValueToGrid('requested', this.controllerStruct.total_request, this.controllerStruct.total_slots, this.baseintervalInitial);
               this.mapValueToGrid('acquisition', this.controllerStruct.acquisition, this.controllerStruct.total_slots, this.baseintervalInitial);

               this.tdmaGeneratorOverflow();

               this.creationInrouteObject();
               this.baseCells();
               this.frameplansqueletorGeneratorChartInrouteName();
               this.frameplansqueletorGeneratorChartRectangle();

               this.tdmaesqueletorGeneratorChartRectangleAxisNumber();

               this.tdmasqueletorGeneratorChartRectangleRequestedRT();
               this.tdmaGeneratorOverflowText();
               this.tdmasqueletorGeneratorChartRectangleRequested();

               this.tdmasqueletorGeneratorChartRectangleRequestedCIR();

               this.tdmasqueletorGeneratorChartRectangleRequestedALLOCATED();
               this.tdmasqueletorGeneratorChartRectangleIDLE();
               this.tdmasqueletorGeneratorChartRectangleFREE();

               this.tdmasqueletorGeneratorChartRectangleDATA();
               this.tdmasqueletorGeneratorChartRectangleACQUISITION();
               this.tdmasqueletorGeneratorChartRectangleZERO();
               this.tdmasqueletorGeneratorChartRectangleOverflowRequested();
               this.tdmasqueletorGeneratorChartRectangleOverflowFrameLayout();

               this.controllerParseData(dataController.data.reply);
               this.stationParseData(dataStations.data.reply);

               this.dataLoaded = true;
            }

         } catch (error) {
         } finally {
            if (item !== 0) {
               //console.log('loop:', item)
               // Schedule the next iteration if the item is not 0
               this.refreshIntervalId = setTimeout(() => {
                  this.executeTask(item);
               }, item);
            }
         }
      },
      async initRequest() {
         this.inrouts = []
         this.dataLoaded = false;
         this.indexRequestRow = 1;
         this.indexQoSRequest = 1;
         this.indexAllocated = 1;
         this.indexFrameLayout = 1;
         this.stationData = []
         const urlController = `${this.$const('CORE_SRV')}/realtime/get/controller${this.$route.params.pathMatch}`;
         const urlStations = `${this.$const('CORE_SRV')}/station/list/controller${this.$route.params.pathMatch}`;
         const dataController = await axios.post(urlController, { command: 'ge 96' })
         const dataControllerCRC = await axios.post(urlController, { command: 'sh st' })
         const dataStations = await axios.post(urlStations, { vars: this.varStation })
         let pathList = [];
         for (let item of dataController.data.path) {
            let url = item.split('_|_')[0];
            let name = item.split('_|_')[1];
            pathList.push({
               'url': url,
               'name': name
            })
         };

         this.$emit('bread-crumbs', pathList);

         let processingPath;
         var preparingPaths = [];
         processingPath = pathList.forEach(element => {
            let pathArr = element.url.split("/");
            let sortedPaths = [];
            sortedPaths = pathArr[3].split(",");
            let name = [];
            let id = [];

            if (sortedPaths.length === 1) {
               name = sortedPaths[0].split("=")[0];
               id = sortedPaths[0].split("=")[1];
               let pathObj = {
                  'name': name,
                  "id": id
               }
               preparingPaths.push(pathObj);
            }
         });
         this.$emit('path-array', preparingPaths);

         if (dataController.data.reply != "No reply") {
            this.parseRealTimeController(dataController.data.reply);
            this.parseStation(dataController.data.reply, dataStations.data.reply, dataControllerCRC.data.reply);
            this.tdmaGenerator();
            this.mapValueToGrid('requested', this.controllerStruct.total_request, this.controllerStruct.total_slots, this.baseintervalInitial);
            this.mapValueToGrid('acquisition', this.controllerStruct.acquisition, this.controllerStruct.total_slots, this.baseintervalInitial);

            this.tdmaGeneratorOverflow();

            this.creationInrouteObject();
            this.baseCells()
            this.frameplansqueletorGeneratorChartInrouteName();
            this.frameplansqueletorGeneratorChartRectangle();

            this.tdmaesqueletorGeneratorChartRectangleAxisNumber();

            this.tdmasqueletorGeneratorChartRectangleRequestedRT()
            this.tdmaGeneratorOverflowText();
            this.tdmasqueletorGeneratorChartRectangleRequested()

            this.tdmasqueletorGeneratorChartRectangleRequestedCIR()

            this.tdmasqueletorGeneratorChartRectangleRequestedALLOCATED();
            this.tdmasqueletorGeneratorChartRectangleIDLE()
            this.tdmasqueletorGeneratorChartRectangleFREE()
            this.tdmasqueletorGeneratorChartRectangleDATA();
            this.tdmasqueletorGeneratorChartRectangleACQUISITION();
            this.tdmasqueletorGeneratorChartRectangleZERO();
            this.tdmasqueletorGeneratorChartRectangleOverflowRequested();
            this.tdmasqueletorGeneratorChartRectangleOverflowFrameLayout();

            this.controllerParseData(dataController.data.reply);
            this.stationParseData(dataStations.data.reply);

            this.dataLoaded = true;
         }


      },
      baseCells() {
         let rowInroute = 5
         let border = '1px solid gray';
         let arrayColumn = []

         this.inrouts.forEach((element, index) => {
            for (let stationIndex = 0; stationIndex < this.controllerStruct.slots_per_frame; stationIndex++) {
               let objColumnConfig = null;
               objColumnConfig = {
                  'grid-area': `${rowInroute} / ${stationIndex + 1} / ${rowInroute} /${stationIndex + 1}`,
                  'border-right': border,
                  'color': 'white'
               }
               arrayColumn.push(objColumnConfig)
            }
            rowInroute += 4
         });
         this.arrayCellColumn = arrayColumn

         return arrayColumn
      },
      creationInrouteObject() {

         for (let index = 0; index < this.controllerStruct.total_channels; index++) {
            let obj = {
               'id': index,
               'name': `Inroute ${index + 1}`,
            };
            this.inrouts.push(obj)
         }
         this.repeatGridColumnFramePlan = this.controllerStruct.slots_per_frame
         this.moduleDivisor = 16;
      },
      controllerParseData(dataController) {
         this.rowDataTopRequested = [];
         let objtStation = {
            id: 1,
            name: this.name_controller,
            inroute: this.controllerStruct.total_channels,
            rq_realtime: this.controllerStruct.total_reques_rt,
            rq_total: this.controllerStruct.total_request,
            rq_allocated: this.controllerStruct.allocated,
            allocated: this.controllerStruct.total_channels * this.controllerStruct.slots_per_frame,
            cir: this.controllerStruct.total_request_cir
         }
         this.rowDataTopRequested.push(objtStation);
         this.gridOptions1.getRowNodeId = (data) => {
            //console.log('gridOptions1:', data.id)
            return data.id;
         };

      },
      stationParseData(dataStations) {

         this.rowDataTopAllocated = [];

         this.stationData.forEach(element => {
            const result = dataStations.list.find(row => row[6] === element.id)?.[0];
            if (element.requested_rt != -2) {
               let objtStation = {
                  id: element.id,
                  name: result.split('_|_')[1],
                  inroute: element.inroute,
                  rq_realtime: element.requested_rt,
                  rq_total: element.sum_requested,
                  allocated: element.allocated,
                  cir: element.request_cir,
                  rq_allocated: element.rq_allocated
               }
               this.rowDataTopAllocated.push(objtStation)
            }

         });

         this.gridOptions2.getRowNodeId = (data) => {
            return data.id;
         };

      },
      arrow_request() {

         if (
            this.overflowcheck_total_request(this.controllerStruct.total_reques_rt) &&
            this.overflowcheck_total_request(this.controllerStruct.total_request)
         ) {
            return false;
         }
         return true;
      },

      parseRealTimeController(data) {
         let arrs = data.split('\r\n')[0].split(',')
         this.controllerStruct.total_channels = parseInt(arrs[0], 10);
         this.controllerStruct.slots_per_frame = parseInt(arrs[1], 10);
         this.controllerStruct.number_stations = parseInt(arrs[2], 10);
         this.controllerStruct.total_request = parseInt(arrs[3], 10);
         this.controllerStruct.total_reques_rt = parseInt(arrs[4], 10);
         this.controllerStruct.total_request_cir = parseInt(arrs[5], 10);
         this.controllerStruct.total_slots = this.controllerStruct.total_channels * this.controllerStruct.slots_per_frame;
      },
      // (stationList[0]//request_cir, stationList[1]//sum_requested, stationList[2]//requested_rt, stationList[3]//allocated, index)

      colorsByStation(CIR_request, total_request, realtime_request, allocated, crc, id) {
         id = id += 1;
         //console.log('arrStationsCRC:', id, crc)

         let listColors = []
         if (crc) {
            // mark_station_as_down(station)  # Black squares
            listColors = [
               { 'amount': allocated, 'color': 'orange', 'name': "CRC" },
            ]
            //console.log(`id=${id}\nDown=${allocated}\n`)

         }
         else if (realtime_request == -1) {
            // mark_station_as_down(station)  # Black squares
            listColors = [
               { 'amount': allocated, 'color': 'gray', 'name': "Down" },
            ]
            //console.log(`id=${id}\nDown=${allocated}\n`)

         }

         else if (realtime_request == -2) {
            //mark_station_as_off(station)  # Gray squares
            listColors = [
               { 'amount': allocated, 'color': 'gray', 'name': "Off" },
            ]
            //console.log(`id=${id}\nOff=${allocated}\n`)
         }
         else if (total_request > 2 * allocated) {
            //mark_station_as_off(station)  # Gray squares
            listColors = [
               { 'amount': allocated, 'color': 'red', 'name': "Overload" },
            ]
            // console.log(`id=${id}\nOverload=${allocated}\n`)
         }
         else {
            let indexCell = 0;
            let CIR_filled = 0;
            let CIR_rt_filled = 0;
            let PIR_filled = 0;
            let PIR_rt_filled = 0;
            let Idle_filled = 0;

            if (CIR_request === 0 && realtime_request === 0) {
               // Caso simple: solo PIR
               PIR_filled = Math.min(total_request, allocated);
               Idle_filled = allocated - total_request;

            } else if (CIR_request > 0 && realtime_request === 0) {
               // Caso sin realtime: CIR y PIR
               CIR_filled = Math.min(CIR_request, allocated);
               PIR_filled = Math.min(total_request - CIR_request, allocated - CIR_filled);
               Idle_filled = allocated - total_request;

            } else {
               if (CIR_request === 0) {
                  // Caso con realtime pero sin CIR
                  PIR_rt_filled = Math.min(realtime_request, allocated);
                  PIR_filled = Math.min(total_request - realtime_request, allocated - PIR_rt_filled);
                  Idle_filled = allocated - total_request;

               } else if (realtime_request > 0) {
                  // CIR realtime gets priority
                  CIR_rt_filled = Math.min(realtime_request, CIR_request);

                  // If there's remaining CIR after realtime
                  CIR_filled = Math.max(0, Math.min(
                     CIR_request - CIR_rt_filled,  // remaining CIR after realtime
                     allocated - CIR_rt_filled     // remaining allocation
                  ));

                  // Any remaining realtime goes to PIR realtime
                  PIR_rt_filled = Math.max(0, Math.min(
                     realtime_request - CIR_rt_filled,  // remaining realtime requests
                     allocated - CIR_rt_filled - CIR_filled  // remaining allocation
                  ));

                  // Regular PIR data
                  PIR_filled = Math.max(0, Math.min(
                     total_request - realtime_request - (CIR_filled + CIR_rt_filled),  // non-realtime requests
                     allocated - CIR_rt_filled - CIR_filled - PIR_rt_filled  // remaining allocation
                  ));

                  Idle_filled = allocated - total_request;
               }
            }
            // Allocate CIRRT slots (highest priority, real-time traffic)
            if (CIR_rt_filled > 0) {
               let cirrt = {
                  'amount': CIR_rt_filled,
                  'color': 'lightblue',
                  'name': "CIRRT data"
               };
               indexCell += CIR_rt_filled;
               listColors.push(cirrt);
            }

            // Allocate CIR slots (after CIRRT overwrite)
            if (CIR_filled > 0) {
               let cir = {
                  'amount': CIR_filled,
                  'color': 'lightgreen',
                  'name': "CIR data"
               };
               indexCell += CIR_filled;
               listColors.push(cir);
            }

            // Allocate PIRRT slots (real-time PIR traffic)
            if (PIR_rt_filled > 0) {
               let pirrt = {
                  'amount': PIR_rt_filled,
                  'color': 'darkblue',
                  'name': "PIRRT data"
               };
               indexCell += PIR_rt_filled;
               listColors.push(pirrt);
            }

            // Allocate PIR slots
            if (PIR_filled > 0) {
               let pir = {
                  'amount': PIR_filled,
                  'color': 'darkgreen',
                  'name': "PIR data"
               };
               indexCell += PIR_filled;
               listColors.push(pir);
            }

            // Allocate Idle slots (remaining unallocated slots)
            if (Idle_filled > 0) {
               let idle = {
                  'amount': Idle_filled,
                  'color': 'pink',
                  'name': "Idle"
               };
               indexCell += Idle_filled;
               listColors.push(idle);
            }

            // Debug output for validation
            /*             console.log(`id=${id}
                        CIR_request=${CIR_request}
                        total_request=${total_request}
                        realtime_request=${realtime_request}
                        allocated=${allocated}
                        CIR_filled=${CIR_filled}
                        PIR_filled=${PIR_filled}
                        CIR_rt_filled=${CIR_rt_filled}
                        PIR_rt_filled=${PIR_rt_filled}
                        Idle_filled=${Idle_filled}`); */



         }

         return listColors
      },
      parseStation(data, dataStation, crcStation) {
         let arrStations = data.split('\r\n').filter(col => col);
         let arrStationsCRC = crcStation
            .split('\r\n')
            .slice(1, -1)
            .map(line => {
               const parts = line.split('|');
               const station = parseInt(parts[0]);
               let crcOLD = (this.stationDataOld.find(item => item.id === station)?.crc || 0);
               let crcCounterOLD = (this.stationDataOld.find(item => item.id === station)?.crc_setted || 1);
               let crcNew = parseInt(parts[1].trim().split(/\s+/)[1]);
               let crcCounterNew = crcCounterOLD;
               let crcToSet = 0;
               let orange = false;
               if (crcNew == 0) {
                  crcToSet = crcNew;
                  crcCounterNew = 1;
                  orange = false;
               } else {
                  if (crcNew == crcOLD) {
                     if (crcCounterOLD < 3) {
                        //if(station == 5) console.log('if1')
                        crcCounterNew = crcCounterOLD + 1;
                        crcToSet = crcOLD;
                        orange = true;
                     } else {
                        // if(station == 5) console.log('if2')
                        crcToSet = crcOLD;
                        orange = false;
                     }

                  } else {
                     // if(station == 5) console.log('if3')
                     crcToSet = crcNew;
                     crcCounterNew = 1;
                     orange = true;
                  }
               }
               //if(station == 5) console.log(`station=${station} crcToSet=${crcToSet} crcCounterNew=${crcCounterNew} orange=${orange}`)
               return { station, crcToSet, crcCounterNew, orange };
            });

         let allocated = 0;
         let idle = 0;
         let acquisition = 0;
         let stationsList = []
         arrStations.shift();
         this.name_controller = dataStation.list.length > 0 ? dataStation.list[0][7] : '';
         arrStations.forEach((station, index) => {
            let stationList = station.split(',').map(Number)
            let stationStruct = {
               'id': index + 1,
               'inroute': [],
               'request_cir': stationList[0],
               'sum_requested': stationList[1],
               'requested_rt': stationList[2],
               'allocated': stationList[3],
               'rq_allocated': 0,
               'colors': this.colorsByStation(
                  stationList[0],
                  stationList[1],
                  stationList[2],
                  stationList[3],
                  (arrStationsCRC.find(item => item.station === index + 1)?.orange || false),
                  index),
               'crc': (arrStationsCRC.find(item => item.station === index + 1)?.crcToSet || 0),
               'crc_status': (arrStationsCRC.find(item => item.station === index + 1)?.orange || false),
               'crc_setted': (arrStationsCRC.find(item => item.station === index + 1)?.crcCounterNew || 1)
            }
            //console.log(`N:${index+1} \nrequest_cir: ${stationList[0]} sum_requested: ${stationList[1]} requested_rt: ${stationList[2]} \nallocated: ${stationList[3]}`)

            if (stationStruct.requested_rt >= 0) {
               allocated += stationStruct.allocated;
               stationStruct.rq_allocated = stationStruct.allocated;
            }
            if (stationStruct.requested_rt == -1) {
               idle += stationStruct.allocated;
            }
            if (stationStruct.requested_rt == -2) {
               acquisition += stationStruct.allocated;
            }

            stationsList.push(stationStruct)
         })
         this.stationData = stationsList;
         this.controllerStruct.allocated = allocated;
         this.controllerStruct.idle = idle;
         this.controllerStruct.free = this.controllerStruct.total_slots - (this.controllerStruct.allocated + this.controllerStruct.idle);
         this.controllerStruct.data = this.controllerStruct.allocated + this.controllerStruct.free;
         this.controllerStruct.acquisition = acquisition;
         this.controllerStruct.zero = this.controllerStruct.total_slots - (this.controllerStruct.data + this.controllerStruct.acquisition)


         //temporal***


         //temporalfin***

      },
      intervalData(item) {
         if (item != 0) {
            clearTimeout(this.refreshIntervalId);
            this.executeTask(item);
         }
         else {
            clearTimeout(this.refreshIntervalId);
         }

      }
   },
   beforeMount() {
   },
   created() {
      this.initRequest();
      this.intervalData(1000);
   },
   mounted() {
   },
   beforeDestroy() {
      clearTimeout(this.refreshIntervalId);
   },
   unmounted() {
      clearTimeout(this.refreshIntervalId);
   }
}
</script>